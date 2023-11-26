## Check for access to an EC2 transit gateway

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
                "ec2:CreateFlowLogs",
                "ec2:CreateNetworkInsightsPath",
                "ec2:CreateTags",
                "ec2:CreateTransitGateway",
                "ec2:CreateTransitGatewayMulticastDomain",
                "ec2:CreateTransitGatewayPeeringAttachment",
                "ec2:CreateTransitGatewayPolicyTable",
                "ec2:CreateTransitGatewayRouteTable",
                "ec2:CreateTransitGatewayVpcAttachment",
                "ec2:CreateVpnConnection",
                "ec2:DeleteTags",
                "ec2:DeleteTransitGateway",
                "ec2:ModifyTransitGateway"
            ],
            "Resource": "arn:aws:ec2:*:*:transit-gateway/tgw-sensitive"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive transit gateway
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"ec2:DeleteTransitGateway",
				"ec2:ModifyTransitGateway",
				"ec2:CreateTransitGatewayPeeringAttachment"
			],
			"Resource": "arn:aws:ec2:*:*:transit-gateway/tgw-not-sensitive"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive transit gateway
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
			"Resource": "arn:aws:ec2:*:*:transit-gateway/tgw-sensitive"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the CreateTransitGatewayPeeringAttachment action on the sensitive transit gateway
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "ec2:CreateTransitGatewayPeeringAttachment",
			"Resource": "arn:aws:ec2:*:*:transit-gateway/tgw-sensitive"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use all EC2 actions and the sensitive transit gateway is included in the resource wildcard.
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
