## Check that an IAM role trust policy only allows access to a particular IAM SAML identity provider

#### Use case

This reference policy checks for unwanted use of OIDC and SAML federation in IAM role trust policies. The check only passes if an allow-listed SAML provider is used and fails if a SAML provider that is not allow-listed is used or if any OIDC provider is used.

#### Reference policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowSpecificSamlProvider",
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::<ACCOUNT ID>:saml-provider/MyProvider"
      },
      "Action": "sts:AssumeRoleWithSAML"
    },
    {
      "Sid": "AllowOtherSTSActions",
      "Effect": "Allow",
      "Principal": "*",
      "NotAction": [
        "sts:AssumeRoleWithSAML",
        "sts:AssumeRoleWithWebIdentity"
      ]
    }
  ]
}
```

#### Description

The check fails if a candidate policy grants access to any identity provider not listed, whether SAML or OIDC.

```Statement: "AllowSpecificSamlProvider"```

This statement checks that an IAM role trust policy only grants access to a particular SAML identity provider using the ```Principal/Federated``` element of a role's trust policy.  This check looks for the ```sts:AssumeRoleWithSAML``` action. 

Note that you need to specify an account ID in the ```Principal/Federated``` element. You can do this, at scale, by dynamically populating the account ID before running the check. This also assumes that the SAML provider is named the same in every account.

```Statement: AllowOtherSTSActions```

The statement allows all other STS actions, except for ```sts:AssumeRoleWithWebIdentity``` and ```sts:AssumeRoleWithSAML```.  The ```sts:AssumeRoleWithSAML``` action is covered by the ```AllowSpecificSamlProvider``` statement. The ```sts:AssumeRoleWithWebIdentity``` action is excluded and implicitly denied to prevent its use entirely. This statement allows the check to succeed for other STS actions.


#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - allows only the allow-listed SAML provider
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::111111111111:saml-provider/MyProvider"
      },
      "Action": "sts:AssumeRoleWithSAML"
    }
  ]
}
```


###### Candidate policy 2: PASS - allows AssumeRole access to a service principal. A check for AssumeRole can be implemented separately, if needed.
```json
{
	"Version": "2012-10-17",
	"Statement": [{
        "Effect": "Allow",
        "Principal": {
            "Service": "ec2.amazonaws.com"
        },
        "Action": "sts:AssumeRole"
    }]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - allows access to an OIDC provider
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

###### Candidate policy 4: FAIL - allows access to a SAML provider that was not allow-listed in the reference policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::111111111111:saml-provider/NotMyProvider"
      },
      "Action": "sts:AssumeRoleWithSAML"
    }
  ]
}
```


###### Candidate policy 5: FAIL - allows access to all SAML providers, except for NotMyProvider
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "*"
      },
      "Action": "sts:AssumeRoleWithSAML"
    },
     {
      "Effect": "Deny",
      "Principal": {
        "Federated": "arn:aws:iam::111111111111:saml-provider/NotMyProvider"
      },
      "Action": "sts:AssumeRoleWithSAML"
    }
  ]
}
```
