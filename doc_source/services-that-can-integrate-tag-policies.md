# Tag policies and AWS Organizations<a name="services-that-can-integrate-tag-policies"></a>

*Tag policies* are a type of policy in AWS Organizations that can help you standardize tags across resources in your organization's accounts\. For more information about tag policies, see [Tag policies](orgs_manage_policies_tag-policies.md)\. 

Use the following information to help you to help you integrate tag policies with AWS Organizations\.

**Topics**
+ [Service principals used by the service\-linked roles](#integrate-enable-svcprin-tag-policies)
+ [Enabling trusted access for tag policies](#integrate-enable-ta-tag-policies)
+ [Disabling trusted access with tag policies](#integrate-disable-ta-tag-policies)

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-tag-policies"></a>

Organizations interacts with the tags attached to your resources using the following service principal:
+ `tagpolicies.tag.amazonaws.com`

## Enabling trusted access for tag policies<a name="integrate-enable-ta-tag-policies"></a>

You can enable trusted access for tag policies by enabling the tag policy type in the AWS Organizations console\. For more information, see [Enabling a policy type](orgs_manage_policies_enable-disable.md#enable-policy-type)\. 

## Disabling trusted access with tag policies<a name="integrate-disable-ta-tag-policies"></a>

You can disable trusted access for tag policies by disabling the tag policy type in the AWS Organizations console\. For more information, see [Disabling a policy type](orgs_manage_policies_enable-disable.md#disable-policy-type)\. 