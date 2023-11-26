## Check for access to an IAM policy

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
                "iam:CreatePolicy",
                "iam:CreatePolicyVersion",
                "iam:DeletePolicy",
                "iam:DeletePolicyVersion",
                "iam:GenerateServiceLastAccessedDetails",
                "iam:GetPolicy",
                "iam:GetPolicyVersion",
                "iam:ListEntitiesForPolicy",
                "iam:ListPolicyTags",
                "iam:ListPolicyVersions",
                "iam:SetDefaultPolicyVersion",
                "iam:TagPolicy",
                "iam:UntagPolicy"
            ],
            "Resource": "arn:aws:iam::*:policy/MySensitiveIamPolicy"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive IAM policy
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"iam:CreatePolicyVersion",
				"iam:SetDefaultPolicyVersion",
				"iam:DeletePolicy"
			],
			"Resource": "arn:aws:iam::*:policy/MyNotSensitiveIamPolicy"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive IAM policy
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
			"Resource": "arn:aws:iam::*:policy/MySensitiveIamPolicy"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the CreatePolicyVersion action on MySensitiveIamPolicy
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "iam:CreatePolicyVersion",
			"Resource": "arn:aws:iam::*:policy/MySensitiveIamPolicy"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use the DeletePolicy action and MySensitiveIamPolicy is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "iam:DeletePolicy",
			"Resource": "arn:aws:iam::*:policy/*Sensitive*"
		}
	]
}
```

###### Candidate policy 5: FAIL - grants access to use all IAM actions and MySensitiveIamPolicy is included in the resource wildcard.
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
