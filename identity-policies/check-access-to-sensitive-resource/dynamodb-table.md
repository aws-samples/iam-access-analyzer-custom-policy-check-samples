## Check for access to a DynamoDB table

#### Description

This reference policy checks if a candidate policy grants access to any of the listed dynamodb actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "dynamodb:BatchGetItem",
                "dynamodb:BatchWriteItem",
                "dynamodb:ConditionCheckItem",
                "dynamodb:CreateBackup",
                "dynamodb:CreateGlobalTable",
                "dynamodb:CreateTable",
                "dynamodb:CreateTableReplica",
                "dynamodb:DeleteItem",
                "dynamodb:DeleteTable",
                "dynamodb:DeleteTableReplica",
                "dynamodb:EnableKinesisStreamingDesintation",
                "dynamodb:DisableKinesisStreamingDestination",
                "dynamodb:ExportTableToPointInTime",
                "dynamodb:GetItem",
                "dynamodb:ImportTable",
                "dynamodb:PartiQLDelete",
                "dynamodb:PartiQLInsert",
                "dynamodb:PartiQLSelect",
                "dynamodb:PartiQLUpdate",
                "dynamodb:PutItem",
                "dynamodb:Query",
                "dynamodb:RestoreTableFromAwsBackup",
                "dynamodb:RestoreTableFromBackup",
                "dynamodb:RestoreTableToPointInTime",
                "dynamodb:Scan",
                "dynamodb:StartAwsBackupJob",
                "dynamodb:TagResource",
                "dynamodb:UntagResource",
                "dynamodb:UpdateContinuousBackups",
                "dynamodb:UpdateContributorInsights",
                "dynamodb:UpdateGlobalTable",
                "dynamodb:UpdateGlobalTableSettings",
                "dynamodb:UpdateGlobalTableVersion",
                "dynamodb:UpdateItem",
                "dynamodb:UpdateTable",
                "dynamodb:UpdateTableReplicaAutoScaling",
                "dynamodb:UpdateTimeToLive"
            ],
            "Resource": "arn:aws:dynamodb:*:*:table/MySensitiveTable"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive table
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "dynamodb:GetItem",
                "dynamodb:Query",
                "dynamodb:Scan"
            ],
            "Resource": "arn:aws:dynamodb:*:*:table/NotASensitiveTable"
        }
    ]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive table
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "dynamodb:*",
            "Resource": "*"
        }, 
        {
            "Effect": "Deny",
            "Action": "*",
            "Resource": "arn:aws:dynamodb:*:*:table/MySensitiveTable"
        }
    ]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the Scan action on MySensitiveTable
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "dynamodb:Scan",
            "Resource": "arn:aws:dynamodb:*:*:table/MySensitiveTable"
        }
    ]
}
```

###### Candidate policy 4: FAIL - grants access to use the GetItem action and MySensitiveTable is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "dynamodb:GetItem",
            "Resource": "arn:aws:dynamodb:*:*:table/*Sensitive*"
        }
    ]
}
```

###### Candidate policy 5: FAIL - grants access to use all dynamodb actions and MySensitiveTable is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "dynamodb:*",
            "Resource": "*"
        }
    ]
}
```
