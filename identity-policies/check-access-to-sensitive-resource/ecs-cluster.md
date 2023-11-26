## Check for access to an ECS cluster

#### Description

This reference policy checks if a candidate policy grants access to any of the listed ECS actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "ecs:DeleteCluster",
                "ecs:DeregisterContainerInstance",
                "ecs:DescribeClusters",
                "ecs:ExecuteCommand",
                "ecs:ListAttributes",
                "ecs:ListContainerInstances",
                "ecs:ListTagsForResource",
                "ecs:PutClusterCapacityProviders",
                "ecs:RegisterContainerInstance",
                "ecs:SubmitAttachmentStateChanges",
                "ecs:SubmitContainerStateChange",
                "ecs:SubmitTaskStateChange",
                "ecs:TagResource",
                "ecs:UntagResource",
                "ecs:UpdateCluster",
                "ecs:UpdateClusterSettings"
            ],
            "Resource": "arn:aws:ecs:*:*:cluster/MySensitiveCluster"
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
				"ecs:DeleteCluster",
				"ecs:UpdateCluster",
				"ecs:ExecuteCommand"
			],
			"Resource": "arn:aws:ecs:*:*:cluster/NotASensitiveCluster"
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
			"Action": "ecs:*",
			"Resource": "*"
		}, 
		{
			"Effect": "Deny",
			"Action": "*",
			"Resource": "arn:aws:ecs:*:*:cluster/MySensitiveCluster"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the ExecuteCommand action on MySensitiveCluster
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "ecs:ExecuteCommand",
			"Resource": "arn:aws:ecs:*:*:cluster/MySensitiveCluster"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use the DeleteCluster action and MySensitiveCluster is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "ecs:DeleteCluster",
			"Resource": "arn:aws:ecs:*:*:cluster/*Sensitive*"
		}
	]
}
```

###### Candidate policy 5: FAIL - grants access to use all ECS actions and MySensitiveCluster is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "ecs:*",
			"Resource": "*"
		}
	]
}
```
