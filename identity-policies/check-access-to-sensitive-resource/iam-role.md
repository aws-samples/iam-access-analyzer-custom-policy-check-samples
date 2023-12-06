## Check for access to a IAM role

#### Description

This reference policy checks if a candidate policy grants access to any of the listed IAM actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "iam:AttachRolePolicy",
                "iam:CreateRole",
                "iam:CreateServiceLinkedRole",
                "iam:DeleteRole",
                "iam:DeleteRolePermissionsBoundary",
                "iam:DeleteRolePolicy",
                "iam:DeleteServiceLinkedRole",
                "iam:DetachRolePolicy",
                "iam:GenerateServiceLastAccessedDetails",
                "iam:GetContextKeysForPrincipalPolicy",
                "iam:GetRole",
                "iam:GetRolePolicy",
                "iam:GetServiceLinkedRoleDeletionStatus",
                "iam:ListAttachedRolePolicies",
                "iam:ListInstanceProfilesForRole",
                "iam:ListPoliciesGrantingServiceAccess",
                "iam:ListRolePolicies",
                "iam:ListRoleTags",
                "iam:PassRole",
                "iam:PutRolePermissionsBoundary",
                "iam:PutRolePolicy",
                "iam:SimulatePrincipalPolicy",
                "iam:TagRole",
                "iam:UntagRole",
                "iam:UpdateAssumeRolePolicy",
                "iam:UpdateRole",
                "iam:UpdateRoleDescription",
                "sts:AssumeRole",
                "sts:TagSession",
                "sts:SetSourceIdentity",
                "sts:AssumeRoleWithSAML",
                "sts:AssumeRoleWithWebIdentity"
            ],
            "Resource": "arn:aws:iam::*:role/MySensitiveRole"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive role
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iam:AttachRolePolicy",
                "iam:DeleteRole",
                "iam:PassRole"
            ],
            "Resource": "arn:aws:iam::*:role/MyNotSensitiveRole"
        }
    ]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive role
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:*",
            "Resource": "*"
        }, 
        {
            "Effect": "Deny",
            "Action": "*",
            "Resource": "arn:aws:iam::*:role/MySensitiveRole"
        }
    ]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the DeleteRole action on MySensitiveRole
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:DeleteRole",
            "Resource": "arn:aws:iam::*:role/MySensitiveRole"
        }
    ]
}
```

###### Candidate policy 4: FAIL - grants access to use the PassRole action and MySensitiveRole is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "arn:aws:iam:*:*:role/*Sensitive*"
        }
    ]
}
```

###### Candidate policy 5: FAIL - grants access to use all iam actions and MySensitiveRole is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:*",
            "Resource": "*"
        }
    ]
}
```
