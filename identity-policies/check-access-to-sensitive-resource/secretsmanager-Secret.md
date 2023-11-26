## Check for access to a SecretsManager secret

#### Description

This reference policy checks if a candidate policy grants access to any of the listed SecretsManager actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "secretsmanager:CancelRotateSecret",
                "secretsmanager:CreateSecret",
                "secretsmanager:DeleteResourcePolicy",
                "secretsmanager:DeleteSecret",
                "secretsmanager:DescribeSecret",
                "secretsmanager:GetResourcePolicy",
                "secretsmanager:GetSecretValue",
                "secretsmanager:ListSecretVersionIds",
                "secretsmanager:PutResourcePolicy",
                "secretsmanager:PutSecretValue",
                "secretsmanager:RemoveRegionsFromReplication",
                "secretsmanager:ReplicateSecretToRegions",
                "secretsmanager:RestoreSecret",
                "secretsmanager:RotateSecret",
                "secretsmanager:StopReplicationToReplica",
                "secretsmanager:TagResource",
                "secretsmanager:UntagResource",
                "secretsmanager:UpdateSecret",
                "secretsmanager:UpdateSecretVersionStage",
                "secretsmanager:ValidateResourcePolicy"
            ],
            "Resource": "arn:aws:secretsmanager:*:*:secret:MySensitiveSecretId"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive secret
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"secretsmanager:DeleteResourcePolicy",
				"secretsmanager:DeleteSecret",
				"secretsmanager:PutResourcePolicy"
			],
			"Resource": "arn:aws:secretsmanager:*:*:secret:MyNotSensitiveSecretId"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive secret
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "secretsmanager:*",
			"Resource": "*"
		}, 
		{
			"Effect": "Deny",
			"Action": "*",
			"Resource": "arn:aws:secretsmanager:*:*:secret:MySensitiveSecretId"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the DeleteSecret action on MySensitiveSecretId
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "secretsmanager:DeleteSecret",
			"Resource": "arn:aws:secretsmanager:*:*:secret:MySensitiveSecretId"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use the PutResourcePolicy action and MySensitiveSecretId is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "secretsmanager:PutResourcePolicy",
			"Resource": "arn:aws:secretsmanager:*:*:secret:*Sensitive*"
		}
	]
}
```

###### Candidate policy 5: FAIL - grants access to use all secretsmanager actions and MySensitiveSecretId is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "secretsmanager:*",
			"Resource": "*"
		}
	]
}
```
