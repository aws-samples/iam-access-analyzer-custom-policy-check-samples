## Check for access to an EC2 instance

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
                "ec2:AssociateAddress",
                "ec2:AssociateIamInstanceProfile",
                "ec2:AttachClassicLinkVpc",
                "ec2:AttachNetworkInterface",
                "ec2:AttachVolume",
                "ec2:CreateFleet",
                "ec2:CreateImage",
                "ec2:CreateInstanceExportTask",
                "ec2:CreateNetworkInsightsPath",
                "ec2:CreateReplaceRootVolumeTask",
                "ec2:CreateSnapshots",
                "ec2:CreateTags",
                "ec2:DeleteTags",
                "ec2:DescribeInstanceAttribute",
                "ec2:DetachClassicLinkVpc",
                "ec2:DetachNetworkInterface",
                "ec2:DetachVolume",
                "ec2:DisassociateIamInstanceProfile",
                "ec2:GetConsoleOutput",
                "ec2:GetConsoleScreenshot",
                "ec2:GetInstanceUefiData",
                "ec2:GetLaunchTemplateData",
                "ec2:GetPasswordData",
                "ec2:ImportInstance",
                "ec2:ModifyInstanceAttribute",
                "ec2:ModifyInstanceCapacityReservationAttributes",
                "ec2:ModifyInstanceCreditSpecification",
                "ec2:ModifyInstanceEventStartTime",
                "ec2:ModifyInstanceMaintenanceOptions",
                "ec2:ModifyInstanceMetadataOptions",
                "ec2:ModifyInstancePlacement",
                "ec2:ModifyNetworkInterfaceAttribute",
                "ec2:ModifyPrivateDnsNameOptions",
                "ec2:MonitorInstances",
                "ec2:PauseVolumeIO",
                "ec2:RebootInstances",
                "ec2:ReplaceIamInstanceProfileAssociation",
                "ec2:ResetInstanceAttribute",
                "ec2:RunInstances",
                "ec2:SendDiagnosticInterrupt",
                "ec2:SendSpotInstanceInterruptions",
                "ec2:StartInstances",
                "ec2:StopInstances",
                "ec2:TerminateInstances",
                "ec2:UnmonitorInstances"
            ],
            "Resource": "arn:aws:ec2:*:*:instance/i-sensitive"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive instance
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"ec2:StopInstances",
				"ec2:TerminateInstances",
				"ec2:CreateImage"
			],
			"Resource": "arn:aws:ec2:*:*:i-not-sensitive"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive instance
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
			"Resource": "arn:aws:ec2:*:*:instance/i-sensitive"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the TerminateInstances action on the sensitive instance
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "ec2:TerminateInstances",
			"Resource": "arn:aws:ec2:*:*:instance/i-sensitive"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use all EC2 actions and the sensitive instance is included in the resource wildcard.
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
