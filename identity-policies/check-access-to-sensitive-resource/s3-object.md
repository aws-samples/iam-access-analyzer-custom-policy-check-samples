## Check for access to a S3 object

#### Description

This reference policy checks if a candidate policy grants access to any of the listed S3 actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "s3:AbortMultipartUpload",
                "s3:BypassGovernanceRetention",
                "s3:DeleteObject",
                "s3:DeleteObjectTagging",
                "s3:DeleteObjectVersion",
                "s3:DeleteObjectVersionTagging",
                "s3:GetObject",
                "s3:GetObjectAcl",
                "s3:GetObjectAttributes",
                "s3:GetObjectLegalHold",
                "s3:GetObjectRetention",
                "s3:GetObjectTagging",
                "s3:GetObjectTorrent",
                "s3:GetObjectVersion",
                "s3:GetObjectVersionAcl",
                "s3:GetObjectVersionAttributes",
                "s3:GetObjectVersionForReplication",
                "s3:GetObjectVersionTagging",
                "s3:GetObjectVersionTorrent",
                "s3:InitiateReplication",
                "s3:ListMultipartUploadParts",
                "s3:ObjectOwnerOverrideToBucketOwner",
                "s3:PutObject",
                "s3:PutObjectAcl",
                "s3:PutObjectLegalHold",
                "s3:PutObjectRetention",
                "s3:PutObjectTagging",
                "s3:PutObjectVersionAcl",
                "s3:PutObjectVersionTagging",
                "s3:ReplicateDelete",
                "s3:ReplicateObject",
                "s3:ReplicateTags",
                "s3:RestoreObject"
            ],
            "Resource": "arn:aws:s3:::my-sensitive-bucket/*"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to objects in sensitive bucket
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"s3:DeleteObject",
				"s3:PutObject",
				"s3:GetObject"
			],
			"Resource": "arn:aws:s3:::my-not-sensitive-bucket/*"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to objects in sensitive bucket
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "s3:*",
			"Resource": "*"
		}, 
		{
			"Effect": "Deny",
			"Action": "*",
			"Resource": "arn:aws:s3:::my-sensitive-bucket/*"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the PutObject action on objects in sensitive bucket
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "s3:PutObject",
			"Resource": "arn:aws:s3:::my-sensitive-bucket/*"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use the GetObject action and objects in sensitive bucket are included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:*:*:*sensitive*/*"
		}
	]
}
```

###### Candidate policy 5: FAIL - grants access to use all S3 actions and objects in sensitive bucket are included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "s3:*",
			"Resource": "*"
		}
	]
}
```
