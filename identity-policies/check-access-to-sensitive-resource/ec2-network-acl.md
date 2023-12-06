## Check for access to an EC2 network ACL

#### Description

This reference policy checks if a candidate policy grants access to any of the listed EC2 actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "ec2:CreateNetworkAcl",
                "ec2:CreateNetworkAclEntry",
                "ec2:CreateTags",
                "ec2:DeleteNetworkAcl",
                "ec2:DeleteNetworkAclEntry",
                "ec2:DeleteTags",
                "ec2:ReplaceNetworkAclAssociation",
                "ec2:ReplaceNetworkAclEntry"
            ],
            "Resource": "arn:aws:ec2:*:*:network-acl/acl-sensitive"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive network ACL
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DeleteNetworkAcl",
                "ec2:DeleteNetworkAclEntry",
                "ec2:ReplaceNetworkAclEntry"
            ],
            "Resource": "arn:aws:ec2:*:*:network-acl/not-sensitive"
        }
    ]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive network ACL
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:*",
            "Resource": "*"
        }, 
        {
            "Effect": "Deny",
            "Action": "*",
            "Resource": "arn:aws:ec2:*:*:network-acl/acl-sensitive"
        }
    ]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the DeleteNetworkAclEntry action on the sensitive network ACL
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:DeleteNetworkAclEntry",
            "Resource": "arn:aws:ec2:*:*:network-acl/acl-123456"
        }
    ]
}
```

###### Candidate policy 4: FAIL - grants access to use all EC2 actions and the sensitive network ACL is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:*",
            "Resource": "*"
        }
    ]
}
```
