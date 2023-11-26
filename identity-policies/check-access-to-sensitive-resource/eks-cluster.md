## Check for access to an EKS cluster

#### Description

This reference policy checks if a candidate policy grants access to any of the listed EKS actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "eks:AccessKubernetesApi",
                "eks:AssociateEncryptionConfig",
                "eks:AssociateIdentityProviderConfig",
                "eks:CreateAddon",
                "eks:CreateFargateProfile",
                "eks:CreateNodegroup",
                "eks:DeleteCluster",
                "eks:DeregisterCluster",
                "eks:DescribeCluster",
                "eks:DescribeUpdate",
                "eks:ListAddons",
                "eks:ListFargateProfiles",
                "eks:ListIdentityProviderConfigs",
                "eks:ListNodegroups",
                "eks:ListTagsForResource",
                "eks:ListUpdates",
                "eks:TagResource",
                "eks:UntagResource",
                "eks:UpdateClusterConfig",
                "eks:UpdateClusterVersion"
            ],
            "Resource": "arn:aws:eks:*:*:cluster/MySensitiveCluster"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive cluster
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"eks:AccessKubernetesApi",
				"eks:DeleteCluster",
				"eks:UpdateClusterConfig"
			],
			"Resource": "arn:aws:eks:*:*:cluster/MyNotSensitiveCluster"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive cluster
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "eks:*",
			"Resource": "*"
		}, 
		{
			"Effect": "Deny",
			"Action": "*",
			"Resource": "arn:aws:eks:*:*:cluster/MySensitiveCluster"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the DeleteCluster action on MySensitiveCluster
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "eks:DeleteCluster",
			"Resource": "arn:aws:eks:*:*:cluster/MySensitiveCluster"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use the AccessKubernetesApi action and MySensitiveCluster is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "eks:AccessKubernetesApi",
			"Resource": "arn:aws:eks:*:*:cluster/*Sensitive*"
		}
	]
}
```

###### Candidate policy 5: FAIL - grants access to use all EKS actions and MySensitiveCluster is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "eks:*",
			"Resource": "*"
		}
	]
}
```
