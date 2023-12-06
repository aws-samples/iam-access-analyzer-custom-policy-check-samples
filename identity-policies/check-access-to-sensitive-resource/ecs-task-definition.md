## Check for access to an ECS task definition

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
                "ecs:DeleteTaskDefinitions",
                "ecs:ListTagsForResource",
                "ecs:RunTask",
                "ecs:StartTask",
                "ecs:TagResource",
                "ecs:UntagResource"
            ],
            "Resource": "arn:aws:ecs:*:*:task-definition/MySensitiveTaskDefinitionFamilyName:*"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive task definition
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecs:DeleteTaskDefinitions",
                "ecs:StartTask",
                "ecs:RunTask"
            ],
            "Resource": "arn:aws:ecs:*:*:task-definition/MyNotSensitiveTaskDefinitionFamilyName:*"
        }
    ]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive task definition
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
            "Resource": "arn:aws:ecs:*:*:task-definition/MySensitiveTaskDefinitionFamilyName:*"
        }
    ]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the DeleteTaskDefinitions action on MySensitiveTaskDefinitionFamilyName
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ecs:DeleteTaskDefinitions",
            "Resource": "arn:aws:ecs:*:*:task-definition/MySensitiveTaskDefinitionFamilyName:*"
        }
    ]
}
```

###### Candidate policy 4: FAIL - grants access to use the RunTask action and * is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ecs:RunTask",
            "Resource": "arn:aws:ecs:*:*:task-definition/*Sensitive*:*"
        }
    ]
}
```

###### Candidate policy 5: FAIL - grants access to use all ECS actions and MySensitiveTaskDefinitionFamilyName is included in the resource wildcard.
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
