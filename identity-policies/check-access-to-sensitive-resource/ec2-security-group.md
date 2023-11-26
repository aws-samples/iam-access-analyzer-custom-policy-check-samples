## Check for access to an EC2 security group

#### Description

This reference policy checks if a candidate policy grants access to any of the listed ec2 actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "ec2:ApplySecurityGroupsToClientVpnTargetNetwork",
                "ec2:AttachClassicLinkVpc",
                "ec2:AuthorizeSecurityGroupEgress",
                "ec2:AuthorizeSecurityGroupIngress",
                "ec2:CreateClientVpnEndpoint",
                "ec2:CreateFleet",
                "ec2:CreateInstanceConnectEndpoint",
                "ec2:CreateNetworkInterface",
                "ec2:CreateSecurityGroup",
                "ec2:CreateTags",
                "ec2:CreateVerifiedAccessEndpoint",
                "ec2:CreateVpcEndpoint",
                "ec2:DeleteSecurityGroup",
                "ec2:DeleteTags",
                "ec2:ImportInstance",
                "ec2:ModifyClientVpnEndpoint",
                "ec2:ModifyFleet",
                "ec2:ModifyInstanceAttribute",
                "ec2:ModifyNetworkInterfaceAttribute",
                "ec2:ModifySecurityGroupRules",
                "ec2:ModifyVpcEndpoint",
                "ec2:RequestSpotFleet",
                "ec2:RequestSpotInstances",
                "ec2:RevokeSecurityGroupEgress",
                "ec2:RevokeSecurityGroupIngress",
                "ec2:RunInstances",
                "ec2:UpdateSecurityGroupRuleDescriptionsEgress",
                "ec2:UpdateSecurityGroupRuleDescriptionsIngress"
            ],
            "Resource": "arn:aws:ec2:*:*:security-group/sg-sensitive"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive security group
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"ec2:DeleteSecurityGroup",
				"ec2:ModifySecurityGroupRules"
			],
			"Resource": "arn:aws:ec2:*:*:security-group/sg-not-sensitive"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive security group
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "ec2:*",
			"Resource": "*"
		}, 
		{
			"Effect": "Deny",
			"Action": "*",
			"Resource": "arn:aws:ec2:*:*:security-group/sg-sensitive"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the DeleteSecurityGroup action on the sensitive security group
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "ec2:DeleteSecurityGroup",
			"Resource": "arn:aws:ec2:*:*:security-group/sg-sensitive"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use all EC2 actions and the sensitive security group is included in the resource wildcard.
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
