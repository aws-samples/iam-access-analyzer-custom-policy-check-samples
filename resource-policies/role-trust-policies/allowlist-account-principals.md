## Check that an IAM role trust policy only allows access to a set of AWS accounts or principals

#### Use case

This reference policy checks that a candidate trust policy only grants access to an allowlisted set AWS accounts and principals in those accounts. This enables you to allow builders to create roles, but constrain the use of those roles to the set of accounts and principals specified. If you want a check that ensures that no access is granted outside of your account or organization, you should use [Access Analyzer access previews](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-preview-access-apis.html), which are available at no additional cost.

You should modify the reference policy to specify your set of allowlisted accounts.

#### Reference policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowPrincipalsInTheseAccounts",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "111111111111",
          "222222222222"
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

The check fails if a candidate policy grants access to any AWS account or principal not listed, a service principal, or a wildcard principal. This check passes if a candidate policy grants access to an entire AWS account or to any AWS principal within that account.

```Statement: "AllowPrincipalsInTheseAccounts"```

This statement checks that an IAM role trust policy only grants access to a specific set of AWS accounts and principals in those accounts using the ```Principal``` element of the trust policy. This check looks for the ```sts:AssumeRole``` action.

```Statement: AllowOtherSTSActions```

The statement allows all other STS actions, except for ```sts:AssumeRole```.  The ```sts:AssumeRole``` action is covered by the ```AllowPrincipalsInTheseAccounts``` statement. This allows the check to succeed for any other STS action and only check for policies that allow the ```sts:AssumeRole``` action.


#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - allows an allowlisted AWS account
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


###### Candidate policy 2: PASS - allows a principal within an allowlisted account
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
        "Effect": "Allow",
        "Principal": {
            "AWS": "arn:aws:iam::222222222222:role/MyRole"
        },
        "Action": "sts:AssumeRole"
    }
  ]
}
```

###### Candidate policy 3: PASS - allows AssumeRoleWithWebIdentity access to an OIDC provider. A check for AssumeRoleWithWebIdentity can be implemented separately, if needed.
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

###### Candidate policy 4: FAIL - allows an AWS service principal
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

###### Candidate policy 5: FAIL - allows access to an AWS account that is not allowlisted
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
        "Effect": "Allow",
        "Principal": {
            "AWS": "333333333333"
        },
        "Action": "sts:AssumeRole"
    }
  ]
}
```

###### Candidate policy 6: FAIL - allows access to an AWS principal that is not in an allowlisted account
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
        "Effect": "Allow",
        "Principal": {
            "AWS": "arn:aws:iam::333333333333:role/NotMyRole"
        },
        "Action": "sts:AssumeRole"
    }
  ]
}
```