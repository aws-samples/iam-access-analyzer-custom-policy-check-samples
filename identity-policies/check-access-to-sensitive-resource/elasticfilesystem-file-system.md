## Check for access to an EFS filesystem

#### Description

This reference policy checks if a candidate policy grants access to any of the listed EFS actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "elasticfilesystem:Backup",
                "elasticfilesystem:ClientMount",
                "elasticfilesystem:ClientRootAccess",
                "elasticfilesystem:ClientWrite",
                "elasticfilesystem:CreateAccessPoint",
                "elasticfilesystem:CreateMountTarget",
                "elasticfilesystem:CreateReplicationConfiguration",
                "elasticfilesystem:CreateTags",
                "elasticfilesystem:DeleteFileSystem",
                "elasticfilesystem:DeleteFileSystemPolicy",
                "elasticfilesystem:DeleteMountTarget",
                "elasticfilesystem:DeleteReplicationConfiguration",
                "elasticfilesystem:DeleteTags",
                "elasticfilesystem:DescribeAccessPoints",
                "elasticfilesystem:DescribeBackupPolicy",
                "elasticfilesystem:DescribeFileSystemPolicy",
                "elasticfilesystem:DescribeFileSystems",
                "elasticfilesystem:DescribeLifecycleConfiguration",
                "elasticfilesystem:DescribeMountTargetSecurityGroups",
                "elasticfilesystem:DescribeMountTargets",
                "elasticfilesystem:DescribeReplicationConfigurations",
                "elasticfilesystem:DescribeTags",
                "elasticfilesystem:ListTagsForResource",
                "elasticfilesystem:ModifyMountTargetSecurityGroups",
                "elasticfilesystem:PutBackupPolicy",
                "elasticfilesystem:PutFileSystemPolicy",
                "elasticfilesystem:PutLifecycleConfiguration",
                "elasticfilesystem:Restore",
                "elasticfilesystem:TagResource",
                "elasticfilesystem:UntagResource",
                "elasticfilesystem:UpdateFileSystem"
            ],
            "Resource": "arn:aws:elasticfilesystem:*:*:file-system/MySensitiveFileSystemId"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive filesystem
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"elasticfilesystem:Backup",
				"elasticfilesystem:ClientMount",
				"elasticfilesystem:ClientRootAccess"
			],
			"Resource": "arn:aws:elasticfilesystem:*:*:file-system/MyNotSensitiveFileSystemId"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive filesystem
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "elasticfilesystem:*",
			"Resource": "*"
		}, 
		{
			"Effect": "Deny",
			"Action": "*",
			"Resource": "arn:aws:elasticfilesystem:*:*:file-system/MySensitiveFileSystemId"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the ClientMount action on MySensitiveFileSystemId
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "elasticfilesystem:ClientMount",
			"Resource": "arn:aws:elasticfilesystem:*:*:file-system/MySensitiveFileSystemId"
		}
	]
}
```

###### Candidate policy 3: FAIL - grants access to use all EFS actions and the sensitive filesystem is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "elasticfilesystem:*",
			"Resource": "*"
		}
	]
}
```
