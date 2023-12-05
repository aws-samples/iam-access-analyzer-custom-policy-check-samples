## Check that an IAM role trust policy only allows access to a set of AWS service principals

#### Use case

This reference policy checks that a candidate trust policy only grants access to an allowlisted set of AWS services. This enables you to allow builders to create roles, but constrain the use of those roles to the set of AWS services specified.

You should modify the reference policy to specify your set of allowlisted service principals.

#### Reference policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowThisSetOfServicePrincipals",
      "Effect": "Allow",
      "Principal": {
        "Service": [
          "ec2.amazonaws.com",
          "lambda.amazonaws.com",
          "ecs-tasks.amazonaws.com"
        ]
      },
      "Action": "sts:AssumeRole"
    },
    {
      "Sid": "AllowOtherSTSActions",
      "Effect": "Allow",
      "Principal": "*",
      "NotAction": "sts:AssumeRole"
    }
  ]
}
```

#### Description

The check fails if a candidate policy grants access to any AWS service principal not listed, a wildcard principal, an AWS account directly, or an IAM principal.

```Statement: "AllowThisSetOfServicePrincipals"```

This statement checks that an IAM role trust policy only grants access to a specific set of AWS service principals using the ```Principal``` element of the trust policy. This check looks for the ```sts:AssumeRole``` action.

```Statement: AllowOtherSTSActions```

The statement allows all other STS actions, except for ```sts:AssumeRole```.  The ```sts:AssumeRole``` action is covered by the ```AllowThisSetOfServicePrincipals``` statement. This allows the check to succeed for any other STS action and only check for policies that allow the ```sts:AssumeRole``` action.


#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - allows an allowlisted AWS service principal
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
        "Effect": "Allow",
        "Principal": {
            "Service": "ec2.amazonaws.com"
        },
        "Action": "sts:AssumeRole"
    }
  ]
}
```

###### Candidate policy 2: PASS - allows AssumeRoleWithWebIdentity access to an OIDC provider. A check for AssumeRoleWithWebIdentity can be implemented separately, if needed.
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::111111111111:oidc-provider/MyProvider"
      },
      "Action": "sts:AssumeRoleWithWebIdentity"
    }
  ]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - allows access to an AWS service principal that is not allowlisted
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
        "Effect": "Allow",
        "Principal": {
            "Service": "glue.amazonaws.com"
        },
        "Action": "sts:AssumeRole"
    }
  ]
}
```

###### Candidate policy 4: FAIL - allows access to an AWS account directly
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
        "Effect": "Allow",
        "Principal": {
            "AWS": "111111111111"
        },
        "Action": "sts:AssumeRole"
    }
  ]
}
```

###### Candidate policy 4: FAIL - allows access to an AWS principal directly
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
        "Effect": "Allow",
        "Principal": {
            "AWS": "arn:aws:iam::111111111111:role/MyRole"
        },
        "Action": "sts:AssumeRole"
    }
  ]
}
```