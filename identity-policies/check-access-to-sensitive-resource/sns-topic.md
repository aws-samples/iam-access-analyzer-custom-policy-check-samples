## Check for access to a SNS topic

#### Description

This reference policy checks if a candidate policy grants access to any of the listed SNS actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "sns:AddPermission",
                "sns:ConfirmSubscription",
                "sns:CreateTopic",
                "sns:DeleteTopic",
                "sns:GetDataProtectionPolicy",
                "sns:GetTopicAttributes",
                "sns:ListSubscriptionsByTopic",
                "sns:ListTagsForResource",
                "sns:Publish",
                "sns:PutDataProtectionPolicy",
                "sns:RemovePermission",
                "sns:SetTopicAttributes",
                "sns:Subscribe",
                "sns:TagResource",
                "sns:UntagResource"
            ],
            "Resource": "arn:aws:sns:*:*:MySensitiveTopicName"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive topic
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sns:AddPermission",
                "sns:ConfirmSubscription",
                "sns:DeleteTopic"
            ],
            "Resource": "arn:aws:sns:*:*:MyNotSensitiveTopicName"
        }
    ]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive topic
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sns:*",
            "Resource": "*"
        }, 
        {
            "Effect": "Deny",
            "Action": "*",
            "Resource": "arn:aws:sns:*:*:MySensitiveTopicName"
        }
    ]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the ConfirmSubscription action on MySensitiveTopicName
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sns:ConfirmSubscription",
            "Resource": "arn:aws:sns:*:*:MySensitiveTopicName"
        }
    ]
}
```

###### Candidate policy 4: FAIL - grants access to use the DeleteTopic action and MySensitiveTopicName is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sns:DeleteTopic",
            "Resource": "arn:aws:sns:*:*:*Sensitive*"
        }
    ]
}
```

###### Candidate policy 5: FAIL - grants access to use all sns actions and MySensitiveTopicName is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sns:*",
            "Resource": "*"
        }
    ]
}
```
