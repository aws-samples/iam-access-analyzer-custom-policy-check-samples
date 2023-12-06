## Check that a policy only grants access to act on a resource with a tag

#### Description

This reference policy checks that if a candidate policy grants access to an action listed in the reference policy, the access must be limited by a resource tag with the provided key (```use-your-own-tag-key-here```). This reference policy does not look at the value of the tag, just that the policy is constrained by some tag with the provided key. You can add additional actions, but ensure they support the ```aws:ResourceTag``` condition key.


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
        "Action": "ec2:TerminateInstances",
        "Resource": "*",
        "Condition": {
          "Null": {
            "aws:ResourceTag/use-your-own-tag-key-here": "true"
          }
        }
      }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - constrains access to the resource tag provided in the reference policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:TerminateInstances",
            "Resource": "*",  
            "Condition": {
                "StringEquals": {
                    "aws:ResourceTag/use-your-own-tag-key-here": "tag-value"
                }
            }
        }
    ]
}
```

###### Candidate policy 2: PASS - this candidate policy does not grant permission to the TerminateInstances action and does not need to be constrained
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


###### Candidate policy 3: PASS - this candidate policy adds a deny statement to constrain access to the provided tag key
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:TerminateInstances",
            "Resource": "*"
        }, 
        {
            "Effect": "Deny",
            "Action": "ec2:TerminateInstances",
            "Resource": "*",  
            "Condition": {
                "StringNotEquals": {
                    "aws:ResourceTag/use-your-own-tag-key-here": "tag-value"
                }
            }
        }
    ]
}
```


#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 4: FAIL - grants access to use the TerminateInstances action and does not constrain access to a resource tag
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:TerminateInstances",
            "Resource": "*"
        }
    ]
}
```

###### Candidate policy 5: FAIL - grants access to use all EC2 actions, which includes TerminateInstances
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
