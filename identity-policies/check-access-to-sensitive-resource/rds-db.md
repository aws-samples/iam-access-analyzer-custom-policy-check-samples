## Check for access to a RDS database

#### Description

This reference policy checks if a candidate policy grants access to any of the listed RDS actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "rds:AddRoleToDBInstance",
                "rds:AddTagsToResource",
                "rds:ApplyPendingMaintenanceAction",
                "rds:CreateBlueGreenDeployment",
                "rds:CreateDBCluster",
                "rds:CreateDBInstance",
                "rds:CreateDBInstanceReadReplica",
                "rds:CreateDBSnapshot",
                "rds:CreateTenantDatabase",
                "rds:DeleteDBInstance",
                "rds:DeleteTenantDatabase",
                "rds:DeregisterDBProxyTargets",
                "rds:DescribeDBInstanceAutomatedBackups",
                "rds:DescribeDBInstances",
                "rds:DescribeDBLogFiles",
                "rds:DescribeDBProxyTargets",
                "rds:DescribeDBSnapshots",
                "rds:DescribeDbSnapshotTenantDatabases",
                "rds:DescribePendingMaintenanceActions",
                "rds:DescribeTenantDatabases",
                "rds:DescribeValidDBInstanceModifications",
                "rds:DownloadCompleteDBLogFile",
                "rds:DownloadDBLogFilePortion",
                "rds:ListTagsForResource",
                "rds:ModifyActivityStream",
                "rds:ModifyDBInstance",
                "rds:ModifyTenantDatabase",
                "rds:PromoteReadReplica",
                "rds:RebootDBInstance",
                "rds:RemoveRoleFromDBInstance",
                "rds:RemoveTagsFromResource",
                "rds:RestoreDBInstanceFromDBSnapshot",
                "rds:RestoreDBInstanceFromS3",
                "rds:RestoreDBInstanceToPointInTime",
                "rds:StartActivityStream",
                "rds:StartDBInstance",
                "rds:StartDBInstanceAutomatedBackupsReplication",
                "rds:StopActivityStream",
                "rds:StopDBInstance",
                "rds:StopDBInstanceAutomatedBackupsReplication",
                "rds:SwitchoverReadReplica"
            ],
            "Resource": "arn:aws:rds:*:*:db:MySensitiveDbInstanceName"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive database
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"rds:AddRoleToDBInstance",
				"rds:ApplyPendingMaintenanceAction",
				"rds:DeleteDBInstance"
			],
			"Resource": "arn:aws:rds:*:*:db:MyNotSensitiveDbInstanceName"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive database
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "rds:*",
			"Resource": "*"
		}, 
		{
			"Effect": "Deny",
			"Action": "*",
			"Resource": "arn:aws:rds:*:*:db:MySensitiveDbInstanceName"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the ApplyPendingMaintenanceAction action on MySensitiveDbInstanceName
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "rds:ApplyPendingMaintenanceAction",
			"Resource": "arn:aws:rds:*:*:db:MySensitiveDbInstanceName"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use the DeleteDBInstance action and MySensitiveDbInstanceName is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "rds:DeleteDBInstance",
			"Resource": "arn:aws:rds:*:*:db:*Sensitive*"
		}
	]
}
```

###### Candidate policy 5: FAIL - grants access to use all rds actions and MySensitiveDbInstanceName is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "rds:*",
			"Resource": "*"
		}
	]
}
```
