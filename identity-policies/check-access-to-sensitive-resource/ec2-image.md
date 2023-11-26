## Check for access to an EC2 image

#### Description

This reference policy checks if a candidate policy grants access to any of the listed ec2 actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "ec2:CancelImageLaunchPermission",
                "ec2:CopyImage",
                "ec2:CreateFleet",
                "ec2:CreateImage",
                "ec2:CreateReplaceRootVolumeTask",
                "ec2:CreateRestoreImageTask",
                "ec2:CreateStoreImageTask",
                "ec2:CreateTags",
                "ec2:DeleteTags",
                "ec2:DeregisterImage",
                "ec2:DescribeImageAttribute",
                "ec2:DisableFastLaunch",
                "ec2:DisableImage",
                "ec2:DisableImageDeprecation",
                "ec2:EnableFastLaunch",
                "ec2:EnableImage",
                "ec2:EnableImageDeprecation",
                "ec2:ExportImage",
                "ec2:ImportImage",
                "ec2:ModifyFleet",
                "ec2:ModifyImageAttribute",
                "ec2:RegisterImage",
                "ec2:RequestSpotFleet",
                "ec2:RequestSpotInstances",
                "ec2:ResetImageAttribute",
                "ec2:RestoreImageFromRecycleBin",
                "ec2:RunInstances"
            ],
            "Resource": "arn:aws:ec2:*:*:image/ami-sensitive"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive image
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"ec2:RunInstances",
				"ec2:CopyImage",
				"ec2:ExportImage"
			],
			"Resource": "arn:aws:ec2:*:*:image/ami-notsensitive"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive image
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
			"Resource": "arn:aws:ec2:*:*:image/ami-sensitive"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use RunInstances action on sensitive image
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "ec2:RunInstances",
			"Resource": "arn:aws:ec2:*:*:image/ami-sensitive"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use all EC2 actions and the sensitive image is included in the resource wildcard.
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
