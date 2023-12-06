## Check for access to a CloudTrail trail

#### Description

This reference policy checks if a candidate policy grants access to any of the listed CloudTrail actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "cloudtrail:AddTags",
                "cloudtrail:CreateTrail",
                "cloudtrail:DeleteTrail",
                "cloudtrail:GetEventSelectors",
                "cloudtrail:GetInsightSelectors",
                "cloudtrail:GetTrail",
                "cloudtrail:GetTrailStatus",
                "cloudtrail:ListTags",
                "cloudtrail:PutEventSelectors",
                "cloudtrail:PutInsightSelectors",
                "cloudtrail:RemoveTags",
                "cloudtrail:StartLogging",
                "cloudtrail:StopLogging",
                "cloudtrail:UpdateTrail"
            ],
            "Resource": "arn:aws:cloudtrail:*:*:trail/MySensitiveTrail"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive trail
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudtrail:UpdateTrail",
                "cloudtrail:StopLogging",
                "cloudtrail:DeleteTrail"
            ],
            "Resource": "arn:aws:cloudtrail:*:*:trail/NotMySensitiveTrail"
        }
    ]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive trail
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "cloudtrail:*",
            "Resource": "*"
        }, 
        {
            "Effect": "Deny",
            "Action": "*",
            "Resource": "arn:aws:cloudtrail:*:*:trail/MySensitiveTrail"
        }
    ]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the StopLogging action on MySensitiveTrail
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "cloudtrail:StopLogging",
            "Resource": "arn:aws:cloudtrail:*:*:trail/MySensitiveTrail"
        }
    ]
}
```

###### Candidate policy 3: FAIL - grants access to use the DeleteTrail action and MySensitiveTrail is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "cloudtrail:DeleteTrail",
            "Resource": "arn:aws:cloudtrail:*:*:trail/*Sensitive*"
        }
    ]
}
```

###### Candidate policy 4: FAIL - grants access to use all CloudTrail actions and MySensitiveTrail is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "cloudtrail:*",
            "Resource": "*"
        }
    ]
}
```
