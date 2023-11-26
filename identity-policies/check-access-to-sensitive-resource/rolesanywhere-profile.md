## Check for access to a IAM Roles Anywhere profile

#### Description

This reference policy checks if a candidate policy grants access to any of the listed IAM Roles Anywhere actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "rolesanywhere:DeleteProfile",
                "rolesanywhere:DisableProfile",
                "rolesanywhere:EnableProfile",
                "rolesanywhere:GetProfile",
                "rolesanywhere:TagResource",
                "rolesanywhere:UntagResource",
                "rolesanywhere:UpdateProfile"
            ],
            "Resource": "arn:aws:rolesanywhere:*:*:profile/MySensitiveProfileId"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive profile
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"rolesanywhere:DeleteProfile",
				"rolesanywhere:DisableProfile",
				"rolesanywhere:UpdateProfile"
			],
			"Resource": "arn:aws:rolesanywhere:*:*:profile/MyNotSensitiveProfileId"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive profile
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "rolesanywhere:*",
			"Resource": "*"
		}, 
		{
			"Effect": "Deny",
			"Action": "*",
			"Resource": "arn:aws:rolesanywhere:*:*:profile/MySensitiveProfileId"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the DisableProfile action on MySensitiveProfileId
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "rolesanywhere:DisableProfile",
			"Resource": "arn:aws:rolesanywhere:*:*:profile/MySensitiveProfileId"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use all rolesanywhere actions and MySensitiveProfileId is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "rolesanywhere:*",
			"Resource": "*"
		}
	]
}
```
