# Understanding enforcement<a name="orgs_manage_policies_tag-policies-enforcement"></a>

A tag policy can specify that noncompliant tagging operations on specified resource types are *enforced*\. In other words, noncompliant tagging requests on specified resource types are prevented from completing\.

**Important**  
Enforcement has no effect on resources that are created without tags\.

To enforce compliance with tag policies, do one of the following when you [create a tag policy](orgs_manage_policies_tag-policies-create.md):
+ From the **Visual editor** tab, select [**Prevent noncompliant operations for this tag**](orgs_manage_policies_tag-policies-create.md#prevent-noncompliant-operations)\.
+ From the **JSON** tab, use the `enforced_for` field\. For information on tag policy syntax, see [Tag policy syntax and examples](orgs_manage_policies_example-tag-policies.md)\.

Follow these best practices for enforcing compliance with tag policies:
+ **Use caution in enforcing compliance** – Make sure you understand the effects of using tag policies, and follow the recommended workflows described in [Getting started with tag policies](orgs_manage_policies_tag-policies-getting-started.md)\. Test how enforcement works on a test account before expanding it to more accounts\. Otherwise, you could prevent users in your organization's accounts from tagging the resources they need\.
+ **Be aware of what resource types you can enforce on** – You can only enforce compliance with tag policies on [supported resource types](orgs_manage_policies_supported-resources-enforcement.md)\. Resource types that support enforcing compliance are listed when you use the visual editor to build a tag policy\. 
+ **Understand interactions with some services ** – Some AWS services have container\-like groupings of resources that automatically create resources for you, and tags can propagate from a resource in one service to another\. For example, tags on Amazon EC2 Auto Scaling groups and Amazon EMR clusters can automatically propagate to the contained Amazon EC2 instances\. You may have tag policies for Amazon EC2 that are more strict than for Auto Scaling groups or EMR clusters\. If you enable enforcement, the tag policy prevents resources from being tagged and may block dynamic scaling and provisioning\.

The following sections show how you can find non\-compliant resources, and correct them to be compliant\.

## Finding non\-compliant resources for an account<a name="enforcement-finding"></a>

For each account, you can get information about non\-compliant resources\. You should run this command from every Region in which the account has resources\.

To find non\-compliant resources for an account that with a tag policy, you can run the following command when signed in to the account and save the results to a file:

```
$ aws resourcegroupstaggingapi get-resources  --region us-east-1 \
    --include-compliance-details \
    --exclude-compliant-resources > outputfile.txt
```

## Correcting non\-compliant tags in resources<a name="enforcement-correcting"></a>

After finding non\-compliant tags, make corrections using any of the following methods\. You must be signed in to the account that has the resource with non\-compliant tags:
+ Use the console or tagging API operations of the AWS service that created the non\-compliant resources\.
+ Use the AWS Resource Groups [TagResources](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/API_TagResources.html) and [UntagResources](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/API_UntagResources.html) operations to add tags that are compliant with the effective policy or to remove non\-compliant tags\.

## Finding and correcting additional non\-compliance issues<a name="enforcement-addl"></a>

Finding and correcting compliance issues is an iterative process\. Repeat the steps in the two previous sections until the resources you care about are compliant with your tag policy\.

## Generating an organization\-wide compliance report<a name="enforcement-report"></a>

At any time, you can generate a report that lists all tagged resources in the AWS accounts across your organization\. The report shows whether each resource is compliant with the effective tag policy\. Note that it can take up to 48 hours for changes you make to a tag policy or resources to be reflected in the organization\-wide compliance report\. For example, assume that you have a tag policy that defines a new standardized tag for a resource type\. Resources of that type that don't have this tag are shown as compliant in the report for up to 48 hours\. 

You can generate the report from your organization's management account in the `us-east-1` Region, provided that it has access to an Amazon S3 bucket\. The bucket must have an attached bucket policy as shown in [Amazon S3 Bucket Policy for Storing Report](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-prereqs.html#bucket-policy)\. To generate the report, run the following command:

```
$ aws resourcegroupstaggingapi get-compliance-summary --region us-east-1
{
    "SummaryList": [
        {
            "LastUpdated": "2020-06-09T18:40:46Z",
            "NonCompliantResources": 0
        }
    ]
}
```

You can generate one report at a time\. 

This report can take some time to complete\. You can check the status by running the following command:

```
$ aws resourcegroupstaggingapi describe-report-creation --region us-east-1
{
    "Status": "SUCCEEDED"
}
```

After the above command returns `SUCCEEDED`, you can open the report from the Amazon S3 bucket\. 