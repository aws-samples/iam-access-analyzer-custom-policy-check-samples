## Check for access to an IAM OIDC provider

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
                "iam:AddClientIDToOpenIDConnectProvider",
                "iam:CreateOpenIDConnectProvider",
                "iam:DeleteOpenIDConnectProvider",
                "iam:GetOpenIDConnectProvider",
                "iam:ListOpenIDConnectProviderTags",
                "iam:RemoveClientIDFromOpenIDConnectProvider",
                "iam:TagOpenIDConnectProvider",
                "iam:UntagOpenIDConnectProvider",
                "iam:UpdateOpenIDConnectProviderThumbprint"
            ],
            "Resource": "arn:aws:iam::*:oidc-provider/MySensitiveOidcProvider"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive OIDC provider
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iam:AddClientIDToOpenIDConnectProvider",
                "iam:RemoveClientIDFromOpenIDConnectProvider",
                "iam:DeleteOpenIDConnectProvider"
            ],
            "Resource": "arn:aws:iam::*:oidc-provider/MyNotSensitiveOidcProvider"
        }
    ]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive OIDC provider
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
            "Resource": "arn:aws:iam::*:oidc-provider/MySensitiveOidcProvider"
        }
    ]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the DeleteOpenIDConnectProvider action on MySensitiveOidcProvider
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:DeleteOpenIDConnectProvider",
            "Resource": "arn:aws:iam::*:oidc-provider/MySensitiveOidcProvider"
        }
    ]
}
```

###### Candidate policy 4: FAIL - grants access to use the DeleteOpenIDConnectProvider action and MySensitiveOidcProvider is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:DeleteOpenIDConnectProvider",
            "Resource": "arn:aws:iam::*:oidc-provider/*Sensitive*"
        }
    ]
}
```

###### Candidate policy 5: FAIL - grants access to use all IAM actions and MySensitiveOidcProvider is included in the resource wildcard.
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
