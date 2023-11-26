## Check for access to a Lambda function

#### Description

This reference policy checks if a candidate policy grants access to any of the listed Lambda actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "lambda:AddPermission",
                "lambda:CreateAlias",
                "lambda:CreateFunction",
                "lambda:CreateFunctionUrlConfig",
                "lambda:DeleteAlias",
                "lambda:DeleteFunction",
                "lambda:DeleteFunctionCodeSigningConfig",
                "lambda:DeleteFunctionConcurrency",
                "lambda:DeleteFunctionEventInvokeConfig",
                "lambda:DeleteFunctionUrlConfig",
                "lambda:DisableReplication",
                "lambda:EnableReplication",
                "lambda:GetAlias",
                "lambda:GetFunction",
                "lambda:GetFunctionCodeSigningConfig",
                "lambda:GetFunctionConcurrency",
                "lambda:GetFunctionConfiguration",
                "lambda:GetFunctionEventInvokeConfig",
                "lambda:GetFunctionUrlConfig",
                "lambda:GetPolicy",
                "lambda:GetRuntimeManagementConfig",
                "lambda:InvokeAsync",
                "lambda:InvokeFunction",
                "lambda:InvokeFunctionUrl",
                "lambda:ListAliases",
                "lambda:ListFunctionEventInvokeConfigs",
                "lambda:ListFunctionUrlConfigs",
                "lambda:ListProvisionedConcurrencyConfigs",
                "lambda:ListTags",
                "lambda:ListVersionsByFunction",
                "lambda:PublishVersion",
                "lambda:PutFunctionCodeSigningConfig",
                "lambda:PutFunctionConcurrency",
                "lambda:PutFunctionEventInvokeConfig",
                "lambda:PutRuntimeManagementConfig",
                "lambda:RemovePermission",
                "lambda:TagResource",
                "lambda:UntagResource",
                "lambda:UpdateAlias",
                "lambda:UpdateFunctionCode",
                "lambda:UpdateFunctionCodeSigningConfig",
                "lambda:UpdateFunctionConfiguration",
                "lambda:UpdateFunctionEventInvokeConfig",
                "lambda:UpdateFunctionUrlConfig"
            ],
            "Resource": "arn:aws:lambda:*:*:function:MySensitiveFunctionName"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive function
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"lambda:AddPermission",
				"lambda:CreateAlias",
				"lambda:DeleteFunction"
			],
			"Resource": "arn:aws:lambda:*:*:function:MyNotSensitiveFunctionName"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive function
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "lambda:*",
			"Resource": "*"
		}, 
		{
			"Effect": "Deny",
			"Action": "*",
			"Resource": "arn:aws:lambda:*:*:function:MySensitiveFunctionName"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the CreateAlias action on MySensitiveFunctionName
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "lambda:CreateAlias",
			"Resource": "arn:aws:lambda:*:*:function:MySensitiveFunctionName"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use the DeleteFunction action and MySensitiveFunctionName is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "lambda:DeleteFunction",
			"Resource": "arn:aws:lambda:*:*:function:*Sensitive*"
		}
	]
}
```

###### Candidate policy 5: FAIL - grants access to use all lambda actions and MySensitiveFunctionName is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "lambda:*",
			"Resource": "*"
		}
	]
}
```
