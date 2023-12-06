## Check for access to an EC2 subnet

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
                "ec2:AssociateClientVpnTargetNetwork",
                "ec2:AssociateRouteTable",
                "ec2:AssociateSubnetCidrBlock",
                "ec2:AssociateTransitGatewayMulticastDomain",
                "ec2:CreateClientVpnRoute",
                "ec2:CreateFleet",
                "ec2:CreateFlowLogs",
                "ec2:CreateInstanceConnectEndpoint",
                "ec2:CreateNatGateway",
                "ec2:CreateNetworkInterface",
                "ec2:CreateSubnet",
                "ec2:CreateTags",
                "ec2:CreateTransitGatewayVpcAttachment",
                "ec2:CreateVerifiedAccessEndpoint",
                "ec2:CreateVpcEndpoint",
                "ec2:DeleteClientVpnRoute",
                "ec2:DeleteSubnet",
                "ec2:DeleteTags",
                "ec2:DisassociateRouteTable",
                "ec2:DisassociateSubnetCidrBlock",
                "ec2:DisassociateTransitGatewayMulticastDomain",
                "ec2:ImportInstance",
                "ec2:ModifyFleet",
                "ec2:ModifySpotFleetRequest",
                "ec2:ModifySubnetAttribute",
                "ec2:ModifyTransitGatewayVpcAttachment",
                "ec2:ModifyVerifiedAccessEndpoint",
                "ec2:ModifyVpcEndpoint",
                "ec2:ReplaceNetworkAclAssociation",
                "ec2:ReplaceRouteTableAssociation",
                "ec2:RequestSpotFleet",
                "ec2:RequestSpotInstances",
                "ec2:RunInstances"
            ],
            "Resource": "arn:aws:ec2:*:*:subnet/subnet-sensitive"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive subnet
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:ReplaceRouteTableAssociation",
                "ec2:DeleteSubnet",
                "ec2:RunInstances"
            ],
            "Resource": "arn:aws:ec2:*:*:subnet/subnet-not-sensitive"
        }
    ]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive subnet
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
            "Resource": "arn:aws:ec2:*:*:subnet/subnet-sensitive"
        }
    ]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the DeleteSubnet action on the sensitive subnet
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:DeleteSubnet",
            "Resource": "arn:aws:ec2:*:*:subnet/subnet-sensitive"
        }
    ]
}
```

###### Candidate policy 4: FAIL - grants access to use all EC2 actions and the sensitive subnet is included in the resource wildcard.
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
