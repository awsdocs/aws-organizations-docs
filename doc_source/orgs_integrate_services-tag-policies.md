# Tag policies and AWS Organizations<a name="orgs_integrate_services-tag-policies"></a>

*Tag policies* are a type of policy that can help you standardize tags across resources in your organization's accounts\. For more information about tag policies, see [Tag policies](orgs_manage_policies_tag-policies.md)\. 

The following list provides information that is useful to know when you want to enable access for tag policies:
+ **To enable trusted access:** Call the [Organizations::EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html) action or perform the action from the [ Organizations console's](https://console.aws.amazon.com/organizations;) **Settings** page\. 

  **To disable trusted access with AWS Organizations:** Call the [AWSOrganizations::DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html) action or perform the action from the [ Organizations console's](https://console.aws.amazon.com/organizations;) **Settings** page\.

  **Service principal name for tag policies: ** `tagpolicies.tag.amazonaws.com`