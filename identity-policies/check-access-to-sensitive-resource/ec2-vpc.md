## Check for access to an EC2 VPC

#### Description

This reference policy checks if a candidate policy grants access to any of the listed EC2 actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "ec2:AcceptVpcPeeringConnection",
                "ec2:ApplySecurityGroupsToClientVpnTargetNetwork",
                "ec2:AssociateDhcpOptions",
                "ec2:AssociateVpcCidrBlock",
                "ec2:AttachClassicLinkVpc",
                "ec2:AttachInternetGateway",
                "ec2:AttachVpnGateway",
                "ec2:CreateCarrierGateway",
                "ec2:CreateClientVpnEndpoint",
                "ec2:CreateEgressOnlyInternetGateway",
                "ec2:CreateFlowLogs",
                "ec2:CreateLocalGatewayRouteTableVpcAssociation",
                "ec2:CreateNetworkAcl",
                "ec2:CreateRouteTable",
                "ec2:CreateSecurityGroup",
                "ec2:CreateSubnet",
                "ec2:CreateTags",
                "ec2:CreateTransitGatewayVpcAttachment",
                "ec2:CreateVpc",
                "ec2:CreateVpcEndpoint",
                "ec2:CreateVpcPeeringConnection",
                "ec2:DeleteTags",
                "ec2:DeleteVpc",
                "ec2:DescribeVpcAttribute",
                "ec2:DetachClassicLinkVpc",
                "ec2:DetachInternetGateway",
                "ec2:DetachVpnGateway",
                "ec2:DisableVpcClassicLink",
                "ec2:DisableVpcClassicLinkDnsSupport",
                "ec2:DisassociateVpcCidrBlock",
                "ec2:EnableVpcClassicLink",
                "ec2:EnableVpcClassicLinkDnsSupport",
                "ec2:GetSecurityGroupsForVpc",
                "ec2:ModifyClientVpnEndpoint",
                "ec2:ModifyVpcAttribute",
                "ec2:ModifyVpcTenancy"
            ],
            "Resource": "arn:aws:ec2:*:*:vpc/vpc-sensitive"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive VPC
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"ec2:AttachInternetGateway",
				"ec2:DeleteVpc",
				"ec2:CreateVpcPeeringConnection"
			],
			"Resource": "arn:aws:ec2:*:*:vpc/vpc-not-sensitive"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive VPC
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
			"Resource": "arn:aws:ec2:*:*:vpc/vpc-sensitive"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the AttachInternetGateway action on the sensitive VPC
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "ec2:AttachInternetGateway",
			"Resource": "arn:aws:ec2:*:*:vpc/vpc-sensitive"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use all EC2 actions and the sensitive VPC is included in the resource wildcard.
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
