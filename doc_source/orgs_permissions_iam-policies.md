# Using Identity\-Based Policies \(IAM Policies\) for AWS Organizations<a name="orgs_permissions_iam-policies"></a>

As an administrator of the master account of an organization, you can control access to AWS resources by attaching permissions policies to IAM identities \(users, groups, and roles\) within the organization\. When granting permissions, you decide who is getting the permissions, the resources they get permissions for, and the specific actions that you want to allow on those resources\. If the permissions are granted to a role, that role can be assumed by users in other accounts in the organization\.

By default, a user has no permissions of any kind\. All permissions must be explicitly granted by a policy\. If a permission is not explicitly granted, then it is implicitly denied\. If a permission is explicitly denied, then that overrules any other policy that might have allowed it\. In other words, a user has only those permissions that are explicitly granted and that are not explicitly denied\.

## Granting Full Admin Permissions to a User<a name="orgs_permissions_grant-admin-actions"></a>

Complete the following steps to grant full AWS Organizations administrator permissions to an IAM user in your organization\.

**To grant full admin permissions to an IAM user in your organization**

1. Sign in to the Identity and Access Management \(IAM\) console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\. Sign in as a user in the master account who has permissions to create IAM policies and attach them to other IAM users\.

   In the IAM console, navigate to **Policies**, **Create Policy**, **Create Your Own Policy**\.

1. Specify a policy name and description in **Policy Name**, **Description**, and then copy and paste the following code into the **Policy Document** field:

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

   This policy enables the user to perform **any **operation associated with the AWS Organizations service\. 

1. Navigate to **Groups**, and then choose **Create New Group**\.

1. On the **Set Group Name** page, type a name for the group, such as `OrganizationAdmins`, and then choose **Next Step**\.

1. On the **Attach Policy** page, select the policy that you just created\. You can filter the list by setting **Policy Type** to **Customer Managed Policies**, or by typing the first few letters of the policy name in the filter box\. Choose **Next Step**, and then **Create Group**\.

1. Choose the new group from the list, and then on the **Users** tab, choose **Add Users to Group**\.

1. Choose the users that you want to grant administrator permissions to, and then choose **Add Users**\.

## Granting Limited Access by Actions<a name="orgs_permissions_grant-limited-actions"></a>

If you want to grant limited permissions instead of full permissions, you can create a policy that lists individual permissions that you want to allow in the "Action" element of the IAM permissions policy\. As shown in the following example, you can use wildcard \(\*\) characters to grant only the `Describe*` and `List*` permissions, essentially providing read\-only access to the organization\.

**Note**  
In an SCPs the wildcard \(\*\) character in an `Action` element can be used only by itself or at the end of the string\. It cannot appear at the beginning or middle of the string\. Therefore, `"servicename:action*"` is valid, but `"servicename:*action"` and `"servicename:some*action"` are both invalid in SCPs\.

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

For a list of all the permissions that are available to assign in an IAM policy, see [Resources, Permissions, and Context Keys You Can Use in an IAM Policy for AWS Organizations](orgs_reference_iam-permissions.md)\.

## Granting Limited Access to Resources<a name="orgs_permissions_grant-limited-resources"></a>

In addition to restricting access to specific actions, you also can restrict access to specific entities in your organization\. The `Resource` elements in the examples in the preceding sections both specify the wildcard character \("\*"\), which means "any resource that the action can access"\. Instead, you can replace the "\*" with the Amazon Resource Name \(ARN\) of specific entities to which you want to allow access\. 

**Example: Granting permissions to a single OU**  
The first statement of the following policy allows an IAM user read access to the entire organization, but the second statement allows the user to perform Organizations administrative actions only within a single, specified organizational unit \(OU\)\. No billing access is granted\. Note that this does not give you administrative access to the accounts in the OU; it grants only permissions to perform Organizations operations on the accounts and child OUs within the specified OU:

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

You get the IDs for the OU and the organization from the AWS Organizations console or by calling the `List*` APIs\. The user or group that you apply this policy to can perform any action \(`"organizations:*"`\) on any entity that is contained by the OU\. The OU is identified by the Amazon Resource Name \(ARN\)\. 

For more information about the ARNs for various resources, see [ARN Formats Supported by AWS Organizations](orgs_reference_arn-formats.md)\. 

## Granting the Ability to Enable Trusted Access to Limited Service Principals<a name="orgs_permissions_grant-trusted-access-condition"></a>

You can use the `Condition` element of a policy statement to further limit the circumstances under which the policy statement matches\.

**Example: Granting permissions to enable trusted access to one specified service**  
The following statement shows how you can restrict the ability to enable trusted access to only those services that you specify\. If the user tries to call the API with a different service principal than the one for AWS Single Sign\-On then this policy doesn't match and the request is denied:

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

For more information about the ARNs for various resources, see [ARN Formats Supported by AWS Organizations](orgs_reference_arn-formats.md)\. 