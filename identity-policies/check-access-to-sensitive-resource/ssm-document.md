## Check for access to a SSM document

#### Description

This reference policy checks if a candidate policy grants access to any of the listed SSM actions on the resource specified. You can add or remove as many or as few actions as you'd like to check for. Use the [service authorization reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) to ensure that the actions act on the resource specified in the ```Resource``` element.  You can also choose one or more resources to treat as sensitive.


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
                "ssm:AddTagsToResource",
                "ssm:CreateAssociation",
                "ssm:CreateAssociationBatch",
                "ssm:CreateDocument",
                "ssm:DeleteAssociation",
                "ssm:DeleteDocument",
                "ssm:DescribeAssociation",
                "ssm:DescribeDocument",
                "ssm:DescribeDocumentParameters",
                "ssm:DescribeDocumentPermission",
                "ssm:GetCalendar",
                "ssm:GetCalendarState",
                "ssm:GetDocument",
                "ssm:ListDocumentMetadataHistory",
                "ssm:ListDocumentVersions",
                "ssm:ListTagsForResource",
                "ssm:ModifyDocumentPermission",
                "ssm:PutCalendar",
                "ssm:RemoveTagsFromResource",
                "ssm:SendCommand",
                "ssm:StartSession",
                "ssm:UpdateAssociation",
                "ssm:UpdateAssociationStatus",
                "ssm:UpdateDocument",
                "ssm:UpdateDocumentDefaultVersion",
                "ssm:UpdateDocumentMetadata"
            ],
            "Resource": "arn:aws:ssm:*:*:document/MySensitiveDocumentName"
        }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - does not grant access to sensitive document
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:DeleteAssociation",
                "ssm:DeleteDocument",
                "ssm:SendCommand"
            ],
            "Resource": "arn:aws:ssm:*:*:document/MyNotSensitiveDocumentName"
        }
    ]
}
```

###### Candidate policy 2: PASS - explicitly denies access to sensitive document
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ssm:*",
            "Resource": "*"
        }, 
        {
            "Effect": "Deny",
            "Action": "*",
            "Resource": "arn:aws:ssm:*:*:document/MySensitiveDocumentName"
        }
    ]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 3: FAIL - grants access to use the DeleteDocument action on MySensitiveDocumentName
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ssm:DeleteDocument",
            "Resource": "arn:aws:ssm:*:*:document/MySensitiveDocumentName"
        }
    ]
}
```

###### Candidate policy 4: FAIL - grants access to use the SendCommand action and MySensitiveDocumentName is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ssm:SendCommand",
            "Resource": "arn:aws:ssm:*:*:document/*Sensitive*"
        }
    ]
}
```

###### Candidate policy 5: FAIL - grants access to use all ssm actions and MySensitiveDocumentName is included in the resource wildcard.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ssm:*",
            "Resource": "*"
        }
    ]
}
```
