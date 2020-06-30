# Best practices for using backup policies<a name="orgs_manage_policies_backup_best-practices"></a>

AWS recommends the following best practices for using backup policies\.

## Decide on a backup policy strategy<a name="bp-tag-cap"></a>

You can create backup policies in incomplete pieces that are inherited and merged to make a complete policy for each member account\. If you do this, you risk ending up with an effective policy that is not complete if you make a change at one level without carefully considering the change's impact on all accounts below that level\. To prevent this, we recommend that you instead ensure that the backup policies you implement at all levels are complete by themselves\. Treat the parent policies as default policies that can be overridden by settings specified in child policies\. That way, even if a child policy doesn't exist, the inherited policy is complete and uses the default values\. You can control which settings can be added to, changed, or removed by child policies by using the [child control inheritance operators](orgs_manage_policies_inheritance_mgmt.md#child-control-operators)\.

## Validate changes to your backup policies checking using `GetEffectivePolicy`<a name="bp-tag-workflow"></a>

After you make a change to a backup policy, check the effective policies for representative accounts below the level where you made the change\. You can [view the effective policy by using the AWS Management Console](orgs_manage_policies_backup_effective.md), or by using the [GetEffectivePolicy](https://docs.aws.amazon.com/organizations/latest/APIReference/API_GetEffectivePolicy.html) API operation or one of its AWS CLI or AWS SDK variants\. Ensure that the change you made had the intended impact on the effective policy\.

## Start simply and make small changes<a name="bp-tag-rules"></a>

To simplify debugging, start with simple policies and make changes one item at a time\. Validate the behavior and impact of each change before making the next change\. This approach reduces the number of variables you have to account for when an error or unexpected result does happen\.

## Limit the number of plans per policy<a name="bp-tag-educate"></a>

Policies that contain multiple plans are more complicated to troubleshoot because of the larger number of outputs that must all be validated\. Instead, have each policy contain one and only one backup plan to simplify debugging and troubleshooting\. You can then add additional policies with other plans to meet other requirements\. This approach helps keep any issues with a plan isolated to one policy, and it prevents those issues from complicating the troubleshooting of issues with other policies and their plans\.

## Use stack sets to create the required backup vaults and IAM roles<a name="bp-tag-compliance"></a>

Use AWS CloudFormation stack sets integration with Organizations to automatically create the required backup vaults and AWS Identity and Access Management \(IAM\) roles in each of the member accounts in your organization\. You can create a stack set that includes the resources you want automatically available in every AWS account in your organization\. This approach enables you to run your backup plans with assurance that the dependencies are already met\. For more information, see [Create a Stack Set with Self\-Managed Permissions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-getting-started-create.html#create-stack-set-service-managed-permissions) in the *AWS CloudFormation User Guide*\.

## Check your results by reviewing the first backup created in each account<a name="bp-tag-guardrails"></a>

When you make a change to a policy, check the next backup created after that change to ensure the change had the desired impact\. This step goes beyond looking at the effective policy and ensures that AWS Backup interprets your policies and implements the backup plans the way you intended\. 