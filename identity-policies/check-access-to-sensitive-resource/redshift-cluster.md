## Check for access to a Redshift cluster

#### Description

This reference policy checks if a candidate policy grants access to any of the listed Redshift actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


#### Reference policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "redshift:CancelResize",
                "redshift:CreateCluster",
                "redshift:CreateCustomDomainAssociation",
                "redshift:CreateTags",
                "redshift:DeleteCluster",
                "redshift:DeleteCustomDomainAssociation",
                "redshift:DeleteTags",
                "redshift:DescribeLoggingStatus",
                "redshift:DescribeResize",
                "redshift:DescribeTags",
                "redshift:DisableLogging",
                "redshift:DisableSnapshotCopy",
                "redshift:EnableLogging",
                "redshift:EnableSnapshotCopy",
                "redshift:FailoverPrimaryCompute",
                "redshift:ModifyAquaConfiguration",
                "redshift:ModifyCluster",
                "redshift:ModifyClusterDbRevision",
                "redshift:ModifyClusterIamRoles",
                "redshift:ModifyClusterSnapshotSchedule",
                "redshift:ModifyCustomDomainAssociation",
                "redshift:ModifySnapshotCopyRetentionPeriod",
                "redshift:PauseCluster",
                "redshift:RebootCluster",
                "redshift:ResizeCluster",
                "redshift:RestoreFromClusterSnapshot",
                "redshift:RestoreTableFromClusterSnapshot",
                "redshift:ResumeCluster",
                "redshift:RotateEncryptionKey"
            ],
            "Resource": "arn:aws:redshift:*:*:cluster:MySensitiveClusterName"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive cluster
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "redshift:DeleteCluster",
                "redshift:DisableLogging",
                "redshift:ModifyCluster"
            ],
            "Resource": "arn:aws:redshift:*:*:cluster:MyNotSensitiveClusterName"
        }
    ]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive cluster
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "redshift:*",
            "Resource": "*"
        }, 
        {
            "Effect": "Deny",
            "Action": "*",
            "Resource": "arn:aws:redshift:*:*:cluster:MySensitiveClusterName"
        }
    ]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the DisableLogging action on MySensitiveClusterName
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "redshift:DisableLogging",
            "Resource": "arn:aws:redshift:*:*:cluster:MySensitiveClusterName"
        }
    ]
}
```

###### Candidate policy 4: FAIL - grants access to use the ModifyCluster action and MySensitiveClusterName is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "redshift:ModifyCluster",
            "Resource": "arn:aws:redshift:*:*:cluster:*Sensitive*"
        }
    ]
}
```

###### Candidate policy 5: FAIL - grants access to use all redshift actions and MySensitiveClusterName is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "redshift:*",
            "Resource": "*"
        }
    ]
}
```
