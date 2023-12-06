## Check that a policy only grants access to an action if a tag is part of the request

#### Description

This reference policy checks that a candidate policy only grants access to the action(s) listed if a tag is provided as part of the request. The specific tag required in this reference policy is ```use-your-own-tag-key-here```. This reference policy does not look at the value of the tag, just that the policy requires you to specify a tag with the key of ```use-your-own-tag-key-here``` as part of the request. You can add additional actions, but ensure the actions support the ```aws:RequestTag``` condition key.

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
        "Action": "ec2:RunInstances",
        "Resource": "*",
        "Condition": {
          "Null": {
            "aws:RequestTag/use-your-own-tag-key-here": "true"
          }
        }
      }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - the candidate policy requires the tag as part of the RunInstances request
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:RunInstances",
            "Resource": "*",  
            "Condition": {
                "StringEquals": {
                    "aws:RequestTag/use-your-own-tag-key-here": "tag-value"
                }
            }
        }
    ]
}
```

###### Candidate policy 2: PASS - this candidate policy does not grant permission to the RunInstances action and does not need to be constrained
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:DescribeInstanceAttribute",
            "Resource": "*"
        }
    ]
}
```

###### Candidate policy 3: PASS - this candidate policy adds a deny statement to constrain access to the provided tag key
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:RunInstances",
            "Resource": "*"
        }, 
        {
            "Effect": "Deny",
            "Action": "ec2:RunInstances",
            "Resource": "*",  
            "Condition": {
                "StringNotEquals": {
                    "aws:RequestTag/use-your-own-tag-key-here": "tag-value"
                }
            }
        }
    ]
}
```


###### Candidate policy 4: PASS - this candidate policy requires a tag with the same key as the reference policy, but does not require a specific value
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:RunInstances",
            "Resource": "*",  
            "Condition": {
                "Null": {
                    "aws:RequestTag/use-your-own-tag-key-here": "false"
                }
            }
        }
    ]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 5: FAIL - grants access to use the RunInstances action and does not require the tag as part of the request
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:RunInstances",
            "Resource": "*"
        }
    ]
}
```

###### Candidate policy 6: FAIL - grants access to use all EC2 actions, which includes RunInstances
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
