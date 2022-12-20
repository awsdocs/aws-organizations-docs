# Using identity\-based policies \(IAM policies\) for AWS Organizations<a name="orgs_permissions_iam-policies"></a>

As an administrator of the management account of an organization, you can control access to AWS resources by attaching permissions policies to AWS Identity and Access Management \(IAM\) identities \(users, groups, and roles\) within the organization\. When granting permissions, you decide who is getting the permissions, the resources they get permissions for, and the specific actions that you want to allow on those resources\. If the permissions are granted to a role, that role can be assumed by users in other accounts in the organization\.

By default, a user has no permissions of any kind\. All permissions must be explicitly granted by a policy\. If a permission isn't explicitly granted, it's implicitly denied\. If a permission is explicitly denied, that overrules any other policy that might have allowed it\. In other words, a user has only those permissions that are explicitly granted and that aren't explicitly denied\.

In addition to the basic techniques described in this topic, you can control access to your organization by using the tags applied to the resources in your organization: the organization root, organizational units \(OU\), accounts, and policies\. For more information, see [Attribute\-based access control with tags and AWS Organizations](orgs_tagging_abac.md)\.

## Granting full admin permissions to a user<a name="orgs_permissions_grant-admin-actions"></a>

You can create an IAM policy that grants full AWS Organizations administrator permissions to an IAM user in your organization\. You can do this using the JSON policy editor in the IAM console\. 

**To use the JSON policy editor to create a policy**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation column on the left, choose **Policies**\. 

   If this is your first time choosing **Policies**, the **Welcome to Managed Policies** page appears\. Choose **Get Started**\.

1. At the top of the page, choose **Create policy**\.

1. Choose the **JSON** tab\.

1. Enter the following JSON policy document:

   ```
   {
       "Version": "2012-10-17",
       "Statement": {
           "Effect": "Allow",
           "Action": "organizations:*",
           "Resource": "*"
       }
   }
   ```

1. Choose **Review policy**\.
**Note**  
You can switch between the **Visual editor** and **JSON** tabs any time\. However, if you make changes or choose **Review policy** in the **Visual editor** tab, IAM might restructure your policy to optimize it for the visual editor\. For more information, see [Policy restructuring](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_policies.html#troubleshoot_viseditor-restructure) in the *IAM User Guide*\.

1. On the **Review policy** page, enter a **Name** and an optional **Description** for the policy that you are creating\. Review the policy **Summary** to see the permissions that are granted by your policy\. Then choose **Create policy** to save your work\.

To learn more about creating an IAM policy, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\.

## Granting limited access by actions<a name="orgs_permissions_grant-limited-actions"></a>

If you want to grant limited permissions instead of full permissions, you can create a policy that lists individual permissions that you want to allow in the `Action` element of the IAM permissions policy\. As shown in the following example, you can use wildcard \(\*\) characters to grant only the `Describe*` and `List*` permissions, essentially providing read\-only access to the organization\.

**Note**  
In a service control policy \(SCP\), the wildcard \(\*\) character in an `Action` element can be used only by itself or at the end of the string\. It can't appear at the beginning or middle of the string\. Therefore, `"servicename:action*"` is valid, but `"servicename:*action"` and `"servicename:some*action"` are both invalid in SCPs\.

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Effect": "Allow",
        "Action": [
            "organizations:Describe*", 
            "organizations:List*" 
        ],
        "Resource": "*"
    }
}
```

For a list of all the permissions that are available to assign in an IAM policy, see [Actions Defined by AWS Organizations](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsorganizations.html#awsorganizations-actions-as-permissions) in the *IAM User Guide*\.

## Granting access to specific resources<a name="orgs_permissions_grant-limited-resources"></a>

In addition to restricting access to specific actions, you can restrict access to specific entities in your organization\. The `Resource` elements in the examples in the preceding sections both specify the wildcard character \("\*"\), which means "any resource that the action can access\." Instead, you can replace the "\*" with the Amazon Resource Name \(ARN\) of specific entities to which you want to allow access\. 

**Example: Granting permissions to a single OU**  
The first statement of the following policy allows an IAM user read access to the entire organization, but the second statement allows the user to perform AWS Organizations administrative actions only within a single, specified organizational unit \(OU\)\. This does not extend to any child OUs\. No billing access is granted\. Note that this doesn't give you administrative access to the AWS accounts in the OU\. It grants only permissions to perform AWS Organizations operations on the accounts within the specified OU:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "organizations:Describe*", 
        "organizations:List*" 
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "organizations:*",
      "Resource": "arn:aws:organizations::<masterAccountId>:ou/o-<organizationId>/ou-<organizationalUnitId>"
    }
  ]
}
```

You get the IDs for the OU and the organization from the AWS Organizations console or by calling the `List*` APIs\. The user or group that you apply this policy to can perform any action \(`"organizations:*"`\) on any entity that is directly contained in the specified OU\. The OU is identified by the Amazon Resource Name \(ARN\)\. 

For more information about the ARNs for various resources, see [Resources Defined by AWS Organizations](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsorganizations.html#awsorganizations-resources-for-iam-policies) in the *IAM User Guide*\. 

## Granting the ability to enable trusted access to limited service principals<a name="orgs_permissions_grant-trusted-access-condition"></a>

You can use the `Condition` element of a policy statement to further limit the circumstances where the policy statement matches\.

**Example: Granting permissions to enable trusted access to one specified service**  
The following statement shows how you can restrict the ability to enable trusted access to only those services that you specify\. If the user tries to call the API with a different service principal than the one for AWS IAM Identity Center \(successor to AWS Single Sign\-On\), this policy doesn't match and the request is denied:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "organizations:EnableAWSServiceAccess",
            "Resource": "*",
            "Condition": { 
                "StringEquals" : {
                    "organizations:ServicePrincipal" : "sso.amazonaws.com"
                }
            }
        }
    ]
}
```

For more information about the ARNs for various resources, see [Resources Defined by AWS Organizations](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awsorganizations.html#awsorganizations-resources-for-iam-policies) in the *IAM User Guide*\. 