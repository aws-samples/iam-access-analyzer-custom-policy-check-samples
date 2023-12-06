## Check for access to a IAM SAML provider

#### Description

This reference policy checks if a candidate policy grants access to any of the listed IAM actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "iam:CreateSAMLProvider",
                "iam:DeleteSAMLProvider",
                "iam:GetSAMLProvider",
                "iam:ListSAMLProviderTags",
                "iam:TagSAMLProvider",
                "iam:UntagSAMLProvider",
                "iam:UpdateSAMLProvider"
            ],
            "Resource": "arn:aws:iam::*:saml-provider/MySensitiveSamlProvider"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive SAML provider
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iam:DeleteSAMLProvider",
                "iam:UntagSAMLProvider",
                "iam:UpdateSAMLProvider"
            ],
            "Resource": "arn:aws:iam::*:saml-provider/MyNotSensitiveSamlProvider"
        }
    ]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive SAML provider
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:*",
            "Resource": "*"
        }, 
        {
            "Effect": "Deny",
            "Action": "*",
            "Resource": "arn:aws:iam::*:saml-provider/MySensitiveSamlProvider"
        }
    ]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the UntagSAMLProvider action on MySensitiveSamlProvider
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:UntagSAMLProvider",
            "Resource": "arn:aws:iam::*:saml-provider/MySensitiveSamlProvider"
        }
    ]
}
```

###### Candidate policy 4: FAIL - grants access to use the UpdateSAMLProvider action and MySensitiveSamlProvider is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:UpdateSAMLProvider",
            "Resource": "arn:aws:iam:*:*:saml-provider/*Sensitive*"
        }
    ]
}
```

###### Candidate policy 5: FAIL - grants access to use all iam actions and MySensitiveSamlProvider is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:*",
            "Resource": "*"
        }
    ]
}
```
