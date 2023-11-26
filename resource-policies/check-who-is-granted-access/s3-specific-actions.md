## Check that a resource policy only grants access to specific principals for specific actions

#### Description

This reference policy checks that a resource policy only grants access to the principals that are listed in either the ```Principal``` element or using the ```aws:PrincipalArn``` condition key. This check only looks at specific actions for the service. 

The check fails if a candidate policy grants access to the specific actions to any other principal. This does not guarantee that no other principal has access to the resource, because access may still be granted on an identity based policy. You could combine this with a check on all identity policies to check that no other access is granted to this resource. See [s3-bucket.md](/identity-policies/check-access-to-sensitive-resource/s3-bucket.md) and [s3-object.md](/identity-policies/check-access-to-sensitive-resource/s3-object.md) for details on these checks.

The reference policy and candidate policy examples show an S3 bucket policy, but this check can be applied to any resource policy by modifying the ```Action``` and ```Resource``` appropriately for that service.

The introduction of the ```Principal``` element makes writing these reference policies slightly different than the identity policy reference policies. The ```IgnoreAnythingNotPutObjectAction``` statement uses the ```NotAction``` element to say that any action other than ```s3:PutObject``` should not be checked by this reference policy.

#### Reference policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "AllowPrincipalElement",
        "Effect": "Allow",
        "Principal": {
          "AWS": [
            "arn:aws:iam::111111111111:role/MyPrivilegedRole",
            "arn:aws:iam::111111111111:role/MyPrivilegedRole2"
          ]
        },
        "Action": "s3:PutObject",
        "Resource": "*"
      },
      {
        "Sid": "AllowPrincipalArnKey",
        "Effect": "Allow",
        "Principal": {
          "AWS": "*"
        },
        "Action": "s3:PutObject",
        "Resource": "*",
        "Condition": {
          "ArnLike": {
            "aws:PrincipalArn": [
              "arn:aws:iam::111111111111:role/MyPrivilegedRole",
              "arn:aws:iam::111111111111:role/MyPrivilegedRole2"
            ]
          }
        }
      },
      {
        "Sid": "IgnoreAnythingNotPutObjectAction",
        "Effect": "Allow",
        "Principal": "*",
        "NotAction": "s3:PutObject",
        "Resource": "*"
      }
    ]
}
```

#### Examples of candidate policies that pass the check against the reference policy

###### Candidate policy 1: PASS - allows GetObject access and not PutObject access
```json
{
	"Version": "2012-10-17",
	"Statement": [{
        "Effect": "Allow",
        "Principal": {
            "AWS": "*"
        },
        "Action": "s3:GetObject",
        "Resource": "arn:aws:s3:::my-sensitive-bucket/*",
        "Condition": {
            "ArnLike": {
                "aws:PrincipalArn": "arn:aws:iam::111111111111:role/SomeOtherRole"
            }
        }
    }]
}
```

###### Candidate policy 2: PASS - allows PutObject access to one of the privileged roles using the Principal element 
```json
{
	"Version": "2012-10-17",
	"Statement": [{
        "Effect": "Allow",
        "Principal": {
            "AWS": "arn:aws:iam::111111111111:role/MyPrivilegedRole"
        },
        "Action": "s3:PutObject",
        "Resource": "arn:aws:s3:::my-sensitive-bucket/*"
    }]
}
```

###### Candidate policy 3: PASS - allows PutObject access to one of the privileged roles using the aws:PrincipalArn condition key 
```json
{
	"Version": "2012-10-17",
	"Statement": [{
        "Effect": "Allow",
        "Principal": {
            "AWS": "*"
        },
        "Action": "s3:PutObject",
        "Resource": "arn:aws:s3:::my-sensitive-bucket/*",
        "Condition": {
            "ArnLike": {
                "aws:PrincipalArn": "arn:aws:iam::111111111111:role/MyPrivilegedRole"
            }
        }
    }]
}
```

#### Examples of candidate policies that fail the check against the reference policy

###### Candidate policy 4: FAIL - allows public access to the sensitive bucket's objects
```json
{
	"Version": "2012-10-17",
	"Statement": [{
        "Effect": "Allow",
        "Principal": {
            "AWS": "*"
        },
        "Action": "s3:*",
        "Resource": "arn:aws:s3:::my-sensitive-bucket/*"
    }]
}
```

###### Candidate policy 5: FAIL - grants access to a principal that was not allowlisted in the reference policy in the Principal element
```json
{
	"Version": "2012-10-17",
	"Statement": [{
        "Effect": "Allow",
        "Principal": {
            "AWS": "arn:aws:iam::111111111111:role/MyOtherRole"
        },
        "Action": "s3:PutObject",
        "Resource": "arn:aws:s3:::my-sensitive-bucket/*"
    }]
}
```


###### Candidate policy 6: FAIL - grants access to a principal that was not allowlisted in the reference policy in the aws:PrincipalArn condition key
```json
{
	"Version": "2012-10-17",
	"Statement": [{
        "Effect": "Allow",
        "Principal": {
            "AWS": "*"
        },
        "Action": "s3:PutObject",
        "Resource": "arn:aws:s3:::my-sensitive-bucket/*",
        "Condition": {
            "ArnLike": {
                "aws:PrincipalArn": "arn:aws:iam::111111111111:role/MyOtherRole"
            }
        }
    }]
}
```
