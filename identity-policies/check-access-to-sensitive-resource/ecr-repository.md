## Check for access to an ECR repository

#### Description

This reference policy checks if a candidate policy grants access to any of the listed ECR actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "ecr:BatchCheckLayerAvailability",
                "ecr:BatchDeleteImage",
                "ecr:BatchGetImage",
                "ecr:BatchGetRepositoryScanningConfiguration",
                "ecr:CompleteLayerUpload",
                "ecr:DeleteLifecyclePolicy",
                "ecr:DeleteRepository",
                "ecr:DeleteRepositoryPolicy",
                "ecr:DescribeImageReplicationStatus",
                "ecr:DescribeImageScanFindings",
                "ecr:DescribeImages",
                "ecr:DescribeRepositories",
                "ecr:GetDownloadUrlForLayer",
                "ecr:GetLifecyclePolicy",
                "ecr:GetLifecyclePolicyPreview",
                "ecr:GetRepositoryPolicy",
                "ecr:InitiateLayerUpload",
                "ecr:ListImages",
                "ecr:ListTagsForResource",
                "ecr:PutImage",
                "ecr:PutImageScanningConfiguration",
                "ecr:PutImageTagMutability",
                "ecr:PutLifecyclePolicy",
                "ecr:ReplicateImage",
                "ecr:SetRepositoryPolicy",
                "ecr:StartImageScan",
                "ecr:StartLifecyclePolicyPreview",
                "ecr:TagResource",
                "ecr:UntagResource",
                "ecr:UploadLayerPart"
            ],
            "Resource": "arn:aws:ecr:*:*:repository/MySensitiveRepo"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive repository
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"ecr:PutImage",
				"ecr:BatchDeleteImage",
				"ecr:DeleteRepository"
			],
			"Resource": "arn:aws:ecr:*:*:repository/NotASensitiveRepo"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive repository
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "ecr:*",
			"Resource": "*"
		}, 
		{
			"Effect": "Deny",
			"Action": "*",
			"Resource": "arn:aws:ecr:*:*:repository/MySensitiveRepo"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the BatchDeleteImage action on MySensitiveRepo
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "ecr:BatchDeleteImage",
			"Resource": "arn:aws:ecr:*:*:repository/MySensitiveRepo"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use the BatchGetImage action and MySensitiveRepo is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "ecr:BatchGetImage",
			"Resource": "arn:aws:ecr:*:*:repository/*Sensitive*"
		}
	]
}
```

###### Candidate policy 5: FAIL - grants access to use all ECR actions and MySensitiveRepo is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "ecr:*",
			"Resource": "*"
		}
	]
}
```
