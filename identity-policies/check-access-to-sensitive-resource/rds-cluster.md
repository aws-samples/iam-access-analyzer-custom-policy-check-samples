## Check for access to a RDS cluster

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
                "rds:AddRoleToDBCluster",
                "rds:AddTagsToResource",
                "rds:ApplyPendingMaintenanceAction",
                "rds:BacktrackDBCluster",
                "rds:CreateBlueGreenDeployment",
                "rds:CreateDBCluster",
                "rds:CreateDBClusterEndpoint",
                "rds:CreateDBClusterSnapshot",
                "rds:CreateDBInstance",
                "rds:CreateDBInstanceReadReplica",
                "rds:CreateGlobalCluster",
                "rds:CreateIntegration",
                "rds:DeleteDBCluster",
                "rds:DeregisterDBProxyTargets",
                "rds:DescribeDBClusterAutomatedBackups",
                "rds:DescribeDBClusterBacktracks",
                "rds:DescribeDBClusterEndpoints",
                "rds:DescribeDBClusters",
                "rds:DescribeDBProxyTargets",
                "rds:DescribePendingMaintenanceActions",
                "rds:FailoverDBCluster",
                "rds:FailoverGlobalCluster",
                "rds:ListTagsForResource",
                "rds:ModifyCurrentDBClusterCapacity",
                "rds:ModifyDBCluster",
                "rds:PromoteReadReplicaDBCluster",
                "rds:RebootDBCluster",
                "rds:RemoveFromGlobalCluster",
                "rds:RemoveRoleFromDBCluster",
                "rds:RemoveTagsFromResource",
                "rds:RestoreDBClusterFromS3",
                "rds:RestoreDBClusterFromSnapshot",
                "rds:RestoreDBClusterToPointInTime",
                "rds:StartActivityStream",
                "rds:StartDBCluster",
                "rds:StopActivityStream",
                "rds:StopDBCluster",
                "rds:SwitchoverGlobalCluster"
            ],
            "Resource": "arn:aws:rds:*:*:cluster:MySensitiveClusterName"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive cluster
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"rds:AddRoleToDBCluster",
				"rds:ApplyPendingMaintenanceAction",
				"rds:DeleteDBCluster"
			],
			"Resource": "arn:aws:rds:*:*:cluster:MyNotSensitiveClusterName"
		}
	]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive cluster
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
			"Resource": "arn:aws:rds:*:*:cluster:MySensitiveClusterName"
		}
	]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the DeleteDBCluster action on MySensitiveClusterName
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "rds:DeleteDBCluster",
			"Resource": "arn:aws:rds:*:*:cluster:MySensitiveClusterName"
		}
	]
}
```

###### Candidate policy 4: FAIL - grants access to use the DeleteDBCluster action and MySensitiveClusterName is included in the resource wildcard.
```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "rds:DeleteDBCluster",
			"Resource": "arn:aws:rds:*:*:cluster:*Sensitive*"
		}
	]
}
```

###### Candidate policy 5: FAIL - grants access to use all rds actions and MySensitiveClusterName is included in the resource wildcard.
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
