## Check if a policy grants access to actions that may be used to escalate privileges

#### Description

This reference policy helps to check if a candidate policy grants access to an action that can be used to escalate privileges. The reference policy ignores a specific role path for both the `iam:PassRole` and `sts:AssumeRole` actions. This allows you to create non-privileged roles that can be passed and assumed. You should customize these paths to fit your organization's guidelines.

You can add additional role paths or specific roles to the NotResource element to exclude them from the check.


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
                "iam:PassRole"
            ],
            "NotResource": "arn:aws:iam::*:role/PassableRoles/*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "sts:AssumeRole"
            ],
            "NotResource": "arn:aws:iam::*:role/AssumableRoles/*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "iam:CreateUser",
                "iam:CreateAccessKey",
                "iam:CreateLoginProfile",
                "iam:AttachUserPolicy",
                "iam:PutUserPolicy",
                "iam:UpdateLoginProfile",
                "iam:AttachGroupPolicy",
                "iam:PutGroupPolicy",
                "iam:AddUserToGroup",
                "iam:CreateRole",
                "iam:AttachRolePolicy",
                "iam:PutRolePolicy",
                "iam:CreatePolicyVersion",
                "iam:SetDefaultPolicyVersion",
                "iam:UpdateAssumeRolePolicy"
            ],
            "Resource": "*"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - assumes a role that's in the ignored path
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "arn:aws:iam::111111111111:role/PassableRoles/MyRole"
        }
    ]
}
```

###### Candidate policy 2: PASS - this candidate policy does not grant permission to actions in this policy check
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:RunInstances",
            "Resource": "*"
        }
    ]
}
```


###### Candidate policy 3: PASS - this candidate policy adds a deny statement to constrain access to the assumable role path
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "*"
        }, 
        {
            "Effect": "Deny",
            "Action": "sts:AssumeRole",
            "NotResource": "arn:aws:iam::*:role/AssumableRoles/MyRole"
        }
    ]
}
```


#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 4: FAIL - grants access to pass a role outside of the allowed path
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "arn:aws:iam::111111111111:role/WrongPath/MyRole"
        }
    ]
}
```

###### Candidate policy 5: FAIL - grants access to use an IAM action that can escalate privileges
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:PutRolePolicy",
            "Resource": "arn:aws:iam::111111111111:role/PrivilegedRole"
        }
    ]
}
```
