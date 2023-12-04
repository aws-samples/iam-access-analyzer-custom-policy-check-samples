## Check that an IAM role trust policy only allows access to a particular IAM OIDC identity provider

#### Description

This reference policy checks that an IAM role trust policy only grants access to a particular OIDC identity provider using the ```Principal``` element of the trust policy.  This check looks for the ```sts:AssumeRoleWithWebIdentity``` action. 

This reference policy can help check for unwanted use of federation in IAM role trust policies.

The check fails if a candidate policy grants access to any other identity provider, whether SAML or OIDC. The ```sts:AssumeRoleWithSAML``` action is excluded from the ```AllowOtherSTSActions``` statement to ensure that the check fails if this action is used. 

Note that you need to specify an account ID in the ```Principal/Federated``` element. You can do this, at scale, by dynamically populating the account ID before running the check. This also assumes that the OIDC provider is named the same in every account.

#### Reference policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowSpecificOidcProvider",
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::<ACCOUNT ID>:oidc-provider/MyProvider"
      },
      "Action": "sts:AssumeRoleWithWebIdentity"
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

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - allows only the allow-listed OIDC provider
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

###### Candidate policy 3: FAIL - allows access to a SAML provider
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

###### Candidate policy 4: FAIL - allows access to an OIDC provider that was not allow-listed in the reference policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "graph.facebook.com"
      },
      "Action": "sts:AssumeRoleWithWebIdentity"
    }
  ]
}
```


###### Candidate policy 5: FAIL - allows access to all OIDC providers, except for NotMyProvider
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "*"
      },
      "Action": "sts:AssumeRoleWithWebIdentity"
    },
     {
      "Effect": "Deny",
      "Principal": {
        "Federated": "arn:aws:iam::111111111111:oidc-provider/NotMyProvider"
      },
      "Action": "sts:AssumeRoleWithWebIdentity"
    }
  ]
}
```
