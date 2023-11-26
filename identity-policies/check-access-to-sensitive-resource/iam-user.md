## Check for access to a IAM user

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
                "iam:AttachUserPolicy",
                "iam:ChangePassword",
                "iam:CreateAccessKey",
                "iam:CreateLoginProfile",
                "iam:CreateServiceSpecificCredential",
                "iam:CreateUser",
                "iam:DeactivateMFADevice",
                "iam:DeleteAccessKey",
                "iam:DeleteLoginProfile",
                "iam:DeleteSSHPublicKey",
                "iam:DeleteServiceSpecificCredential",
                "iam:DeleteSigningCertificate",
                "iam:DeleteUser",
                "iam:DeleteUserPermissionsBoundary",
                "iam:DeleteUserPolicy",
                "iam:DetachUserPolicy",
                "iam:EnableMFADevice",
                "iam:GenerateServiceLastAccessedDetails",
                "iam:GetAccessKeyLastUsed",
                "iam:GetContextKeysForPrincipalPolicy",
                "iam:GetLoginProfile",
                "iam:GetMFADevice",
                "iam:GetSSHPublicKey",
                "iam:GetUser",
                "iam:GetUserPolicy",
                "iam:ListAccessKeys",
                "iam:ListAttachedUserPolicies",
                "iam:ListGroupsForUser",
                "iam:ListMFADevices",
                "iam:ListPoliciesGrantingServiceAccess",
                "iam:ListSSHPublicKeys",
                "iam:ListServiceSpecificCredentials",
                "iam:ListSigningCertificates",
                "iam:ListUserPolicies",
                "iam:ListUserTags",
                "iam:PutUserPermissionsBoundary",
                "iam:PutUserPolicy",
                "iam:ResetServiceSpecificCredential",
                "iam:ResyncMFADevice",
                "iam:SimulatePrincipalPolicy",
                "iam:TagUser",
                "iam:UntagUser",
                "iam:UpdateAccessKey",
                "iam:UpdateLoginProfile",
                "iam:UpdateSSHPublicKey",
                "iam:UpdateServiceSpecificCredential",
                "iam:UpdateSigningCertificate",
                "iam:UpdateUser",
                "iam:UploadSSHPublicKey",
                "iam:UploadSigningCertificate",
                "sts:TagSession",
                "sts:SetSourceIdentity",
                "sts:GetFederationToken"
            ],
            "Resource": "arn:aws:iam::*:user/MySensitiveUser"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive user
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"iam:AttachUserPolicy",
				"iam:ChangePassword",
				"iam:CreateAccessKey"
			],
			"Resource": "arn:aws:iam::*:user/MyNotSensitiveUser"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive user
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
			"Resource": "arn:aws:iam::*:user/MySensitiveUser"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the ChangePassword action on MySensitiveUser
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "iam:ChangePassword",
			"Resource": "arn:aws:iam::*:user/MySensitiveUser"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use the CreateAccessKey action and MySensitiveUser is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "iam:CreateAccessKey",
			"Resource": "arn:aws:iam:*:*:user/*Sensitive*"
		}
	]
}
```

###### Candidate policy 5: FAIL - grants access to use all iam actions and MySensitiveUser is included in the resource wildcard.
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
