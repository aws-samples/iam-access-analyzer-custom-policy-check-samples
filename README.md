## AWS IAM Access Analyzer CheckNoNewAccess Sample Reference Policies

This repository contains a collection of sample reference policies that can be used with the Access Analyzer's CheckNoNewAccess API.  The CheckNoNewAccess API checks an existing policy against a new policy and returns PASS if no new access is detected in the new policy and FAIL if new access is detected in the new policy.

The reference policies in this repository are meant to be passed to the ExistingPolicyDocument parameter of the CheckNoNewAccess API.  You then pass the IAM policy you're attempting to check for new access (referred to as the candidate-policy in this repository) in the NewPolicyDocument parameter.  Example:

```
aws accessanalyzer check-no-new-access --existing-policy-document file://reference-policy.json --new-policy-document file://candidate-policy.json --policy-type IDENTITY_POLICY 
```

The CheckNoNewAccess API supports both identity policies and resource policies and is built on Zelkova, which translates IAM policies into equivalent logical statements, and runs a suite of general-purpose and specialized logical solvers (satisfiability modulo theories) against the problem. To learn more about satisfiability modulo theories, see [Satisfiability Modulo Theories](https://people.eecs.berkeley.edu/~sseshia/pubdir/SMT-BookChapter.pdf).

### What are reference policies?

You use a reference policy to check if a candidate policy allows more access than the reference policy does. Worded differently, the check passes if the candidate policy is a subset of the reference policy.  

With properly crafted reference policies, this allows you to make assertions like:

- This policy does not grant access to my sensitive resource.
- Only these principals are allowed to access my sensitive resource.
- This IAM action can only be granted if accompanied by a tagging condition key.

Let's walk through an example. A reference policy typically starts by allowing all access. If you've seen [service control policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html) before, this probably looks familiar to you. You then add a statement or statements that deny the access that you want the reference policy to check for.

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
                "s3:PutBucketPolicy"
            ],
            "Resource": "arn:aws:s3:::my-sensitive-bucket"
        }
    ]
}
```
> Example 1: Reference policy that checks for ```s3:PutBucketPolicy``` access to ```my-sensitive-bucket```

In Example 1 above, we start by allowing all access. We then deny the ```s3:PutBucketPolicy``` action against a specific sensitive bucket, ```my-sensitive-bucket```.  You could choose to add more than one bucket here.

The CheckNoNewAccess API will return a PASS, meaning no new access, for candidate policies that do not grant ```s3:PutBucketPolicy``` access to ```my-sensitive-bucket```.  The API will return a FAIL for candidate policies that do grant access to the sensitive bucket. You might use that result to decide that a policy that grants ```s3:PutBucketPolicy``` access to a sensitive bucket needs additional review as part of a CI/CD pipeline process.

You can use this same type of pattern with any AWS service and there are a number of reference policies that you can customize in this repository.

### What if I just want to check for IAM actions and not deal with reference policies?

You should use the CheckAccessNotGranted API instead. The CheckAccessNotGranted API checks that a candidate policy does not grant access to an IAM action.

### What does it mean when the check fails and there is "new access" when checking an IAM policy against a reference policy?

New access means that the candidate policy you're checking against grants additional access that the reference policy does not grant.

This is why we recommend you start your reference policies with a statement that grants access to all actions and resources.  You then write statements that deny the particular access that you're checking for. This means that the only way your candidate policy could grant access that is not granted in the reference policy is if it grants the access denied by the reference policy.

In Example 1 above, the only way a candidate policy can grant more access than the reference policy is if it allows access to exactly the ```s3:PutBucketPolicy``` action and the ```my-sensitive-bucket``` resource.


### How should I use the example reference policies in this repository?

The example reference policies in this repository are divided into two top-level folders - resource policies and identity policies.

***Identity policies***

- **check-access-to-sensitive-resource** - Checks for access to a sensitive resource. You should replace the placeholder ARN with the ARN of your sensitive resource to the deny statement. You can add more than one resource if you want to check for multiple sensitive resources. You can also add or remove actions depending on the actions you want to check access for.  You can also combine reference policies by appending multiple deny statements to a single allow statement.  Checking access to a sensitive resource is the type of example shown in Example 1 above.
- **check-for-tag-based-access** - Checks that access that uses a certain action is constrained by a tagging condition key.  For example, the reference policy [```run-ec2-instance-with-tag.md```](identity-policies/check-for-tag-based-access/run-ec2-instance-with-tag.md) checks that a policy only allows access to run an EC2 instance if a specific tagging condition key is specified.  The reference policy [```act-on-ec2-instance-with-tag.md```](identity-policies/check-for-tag-based-access/act-on-ec2-instance-with-tag.md) checks that a policy can only grant access to terminate an EC2 instance for a particular resource tag value. You could use this type of reference policy to help enforce certain tag based access control strategies in your development process.


***Resource policies***

- **check-who-is-granted-access** - Checks that a resource policy only allows access to a specific principal or set of principals. Examples suffixed with ```-specific-actions``` only check that specific, potentially sensitive actions are restricted to a principal or principals. Other actions in these examples are not checked. Examples suffixed with ```-all-actions``` check that all actions are only granted to a specific principal or set of principals.

    The introduction of the ```Principal``` element makes writing these reference policies slightly different.  The reference policy allows the principal(s) that can be granted access, using both the ```Principal``` element and ```aws:PrincipalArn``` condition key. For the examples that only check for specific actions, add an extra statement to allow all remaining actions using the ```NotAction``` element. See [```s3-specific-actions.md```](resource-policies/check-who-is-granted-access/s3-specific-actions.md) for an example.



### How do I write my own reference policies?

We recommend sticking to the examples provided in this repository and customizing them as you see fit. If you do want to write your own reference policies, you should follow these general patterns.

For identity policies, start with a statement that allows all actions and resources. Add a deny statement for actions, resources, and conditions that should cause the check to fail if they are granted in a candidate policy.

For resource policies, add statements to allow access to the principals, actions, resources, and conditions as you normally would when writing a resource policy.  If you only want to check for specific actions or resources, add a statement with a ```NotAction``` or ```NotResource``` element at the end that includes the specific actions and resources you're checking for and effectively allows all other actions and resources.


### What are some things to keep in mind when writing my own reference policies?

If you go the path of writing your own reference policies, we recommend customizing the reference policies in this repository or following the general patterns described above. There are some things to be aware of when you customize the existing reference policies or write your own.

- Policy checks do not have knowledge of all relationships between actions and conditions. For example, they do not know if an action only supports a ```*``` resource. This means that you can't write reference policies as shown below in Example 2.

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
                "Action": "s3:*",
                "Resource": "arn:aws:s3:::my-sensitive-bucket"
            }
        ]
    }
    ```
    > Example 2:  A reference policy that does not work for all S3 actions.

    Because S3 has actions that do not support specifying a resource, like ```s3:ListAllMyBuckets```, this reference policy will assume that any candidate policy must specify a resource when allowing access to an S3 action. Since this is not possible, using Example 2 as a reference policy will give you checks that will have false positives.

- Conditions are not organization-aware. This means that policy checks do not know if an account or OU path belongs to your organization. Keep that in mind if you try to use the ```aws:PrincipalOrgId``` or similar condition keys. If you want to check for external or cross-account access, you should use [Access Analyzer access previews](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-preview-access-apis.html).
