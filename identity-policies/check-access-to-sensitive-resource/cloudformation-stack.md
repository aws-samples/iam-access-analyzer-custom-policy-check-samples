## Check for access to a CloudFormation stack

#### Description

This reference policy checks if a candidate policy grants access to any of the listed CloudFormation actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "cloudformation:CancelUpdateStack",
                "cloudformation:ContinueUpdateRollback",
                "cloudformation:CreateChangeSet",
                "cloudformation:CreateStack",
                "cloudformation:DeleteChangeSet",
                "cloudformation:DeleteStack",
                "cloudformation:DescribeChangeSet",
                "cloudformation:DescribeChangeSetHooks",
                "cloudformation:DescribeStackEvents",
                "cloudformation:DescribeStackResource",
                "cloudformation:DescribeStackResourceDrifts",
                "cloudformation:DescribeStackResources",
                "cloudformation:DescribeStacks",
                "cloudformation:DetectStackDrift",
                "cloudformation:DetectStackResourceDrift",
                "cloudformation:ExecuteChangeSet",
                "cloudformation:GetStackPolicy",
                "cloudformation:GetTemplate",
                "cloudformation:GetTemplateSummary",
                "cloudformation:ListChangeSets",
                "cloudformation:ListStackResources",
                "cloudformation:RecordHandlerProgress",
                "cloudformation:RollbackStack",
                "cloudformation:SetStackPolicy",
                "cloudformation:SignalResource",
                "cloudformation:TagResource",
                "cloudformation:UntagResource",
                "cloudformation:UpdateStack",
                "cloudformation:UpdateTerminationProtection"
            ],
            "Resource": "arn:aws:cloudformation:*:*:stack/MySensitiveStack/*"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive stack
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudformation:UpdateStack",
                "cloudformation:DeleteStack",
                "cloudformation:RollbackStack"
            ],
            "Resource": "arn:aws:cloudformation:*:*:stack/NotMySensitiveStack/*"
        }
    ]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive stack
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "cloudformation:*",
            "Resource": "*"
        }, 
        {
            "Effect": "Deny",
            "Action": "*",
            "Resource": "arn:aws:cloudformation:*:*:stack/MySensitiveStack/*"
        }
    ]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the DeleteStack action on stack/MySensitiveStack/*
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "cloudformation:DeleteStack",
            "Resource": "arn:aws:cloudformation:*:*:stack/MySensitiveStack/*"
        }
    ]
}
```

###### Candidate policy 3: FAIL - grants access to use the DeleteStack action and stack/MySensitiveStack/* is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "cloudformation:DeleteStack",
            "Resource": "arn:aws:cloudformation:*:*:stack/*Sensitive*/*"
        }
    ]
}
```

###### Candidate policy 4: FAIL - grants access to use all CloudFormation actions and stack/MySensitiveStack/* is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "cloudformation:*",
            "Resource": "*"
        }
    ]
}
```
