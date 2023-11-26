## Check for access to a SQS queue

#### Description

This reference policy checks if a candidate policy grants access to any of the listed SQS actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "sqs:AddPermission",
                "sqs:CancelMessageMoveTask",
                "sqs:ChangeMessageVisibility",
                "sqs:CreateQueue",
                "sqs:DeleteMessage",
                "sqs:DeleteQueue",
                "sqs:GetQueueAttributes",
                "sqs:GetQueueUrl",
                "sqs:ListDeadLetterSourceQueues",
                "sqs:ListMessageMoveTasks",
                "sqs:ListQueueTags",
                "sqs:PurgeQueue",
                "sqs:ReceiveMessage",
                "sqs:RemovePermission",
                "sqs:SendMessage",
                "sqs:SetQueueAttributes",
                "sqs:StartMessageMoveTask",
                "sqs:TagQueue",
                "sqs:UntagQueue"
            ],
            "Resource": "arn:aws:sqs:*:*:MySensitiveQueueName"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive queue
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"sqs:AddPermission",
				"sqs:DeleteQueue",
				"sqs:DeleteMessage"
			],
			"Resource": "arn:aws:sqs:*:*:MyNotSensitiveQueueName"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive queue
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "sqs:*",
			"Resource": "*"
		}, 
		{
			"Effect": "Deny",
			"Action": "*",
			"Resource": "arn:aws:sqs:*:*:MySensitiveQueueName"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the DeleteQueue action on MySensitiveQueueName
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "sqs:DeleteQueue",
			"Resource": "arn:aws:sqs:*:*:MySensitiveQueueName"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use the DeleteMessage action and MySensitiveQueueName is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "sqs:DeleteMessage",
			"Resource": "arn:aws:sqs:*:*:*Sensitive*"
		}
	]
}
```

###### Candidate policy 5: FAIL - grants access to use all sqs actions and MySensitiveQueueName is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "sqs:*",
			"Resource": "*"
		}
	]
}
```
