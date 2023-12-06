## Check for access to a IAM Roles Anywhere trust anchor

#### Description

This reference policy checks if a candidate policy grants access to any of the listed IAM Roles Anywhere actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "rolesanywhere:DeleteTrustAnchor",
                "rolesanywhere:DisableTrustAnchor",
                "rolesanywhere:EnableTrustAnchor",
                "rolesanywhere:GetTrustAnchor",
                "rolesanywhere:PutNotificationSettings",
                "rolesanywhere:ResetNotificationSettings",
                "rolesanywhere:TagResource",
                "rolesanywhere:UntagResource",
                "rolesanywhere:UpdateTrustAnchor"
            ],
            "Resource": "arn:aws:rolesanywhere:*:*:trust-anchor/MySensitiveTrustAnchorId"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive trust anchor
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "rolesanywhere:DeleteTrustAnchor",
                "rolesanywhere:DisableTrustAnchor",
                "rolesanywhere:UpdateTrustAnchor"
            ],
            "Resource": "arn:aws:rolesanywhere:*:*:trust-anchor/MyNotSensitiveTrustAnchorId"
        }
    ]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive trust anchor
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "rolesanywhere:*",
            "Resource": "*"
        }, 
        {
            "Effect": "Deny",
            "Action": "*",
            "Resource": "arn:aws:rolesanywhere:*:*:trust-anchor/MySensitiveTrustAnchorId"
        }
    ]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the DisableTrustAnchor action on MySensitiveTrustAnchorId
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "rolesanywhere:DisableTrustAnchor",
            "Resource": "arn:aws:rolesanywhere:*:*:trust-anchor/MySensitiveTrustAnchorId"
        }
    ]
}
```

###### Candidate policy 4: FAIL - grants access to use all rolesanywhere actions and MySensitiveTrustAnchorId is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "rolesanywhere:*",
            "Resource": "*"
        }
    ]
}
```
