# Using tag policies in the AWS CLI<a name="tag-policy-cli"></a>

To use tag policies in the AWS Command Line Interface, AWS recommends that you first follow the basic workflow described in this topic\. 

## Recommended workflow<a name="tag-policy-recommended-workflow-cli"></a>

The recommended workflow for using tag policies is as follows:

1. [Enable tag policies for your organization\.](#tag-policy-enable-cli)

1. [Create a tag policy\.](#tag-policy-create-first-cli)

1. [Attach a tag policy to a single, test account\.](#tag-policy-attach-first-cli)

1. [View the effective tag policy for that account\.](#tag-policy-get-effective-cli)

1. [Find and correct noncompliant tags on resources\.](#tag-policy-find-noncompliant-cli)

1. [Find and correct more noncompliant tags on resources\.](#tag-policy-repeat-cli)

1. At any time, you can find out the compliance status for all tagged resources across all accounts across your organization\. To do this, [generate the organization\-wide report\.](#generate-org-wide-report-cli)

## Enabling tag policies for your organization<a name="tag-policy-enable-cli"></a>

Enabling the use of tag policies is a one\-time task\. You enable tag policies on the organization root, even if you plan to attach tag policies to individual accounts only\. 

**To enable tag policies**

1. Find the root ID of your organization so you can specify where to enable and attach a tag policy\. To find the root ID, run the following from a command prompt:

   ```
   aws --region us-east-1 organizations list-roots 
   ```

   The following is an example result:

   ```
   {
       "Roots": [
           {
               "Id": "r-examplerootid111",
               "Arn": "arn:aws:organizations::111111111111:root/o-exampleorgid/r-examplerootid111",
               "Name": "Root",
               "PolicyTypes": []
           }
       ]
   }
   ```

   In this example, `r-examplerootid111` is the root ID of the organization\. You use this root ID in the next step\.

1. Run the following to enable tag policies for the organization:

   ```
   aws --region us-east-1 resourcegroupstaggingapi enable-tag-policies --root-id r-examplerootid111
   ```

   This command enables tag policies for the organization with the root ID `r-examplerootid111.` 

## Creating a tag policy<a name="tag-policy-create-first-cli"></a>

After you enable tag policies, you're ready to create your first tag policy\. 

You can use any text editor to create a tag policy\. Use JSON syntax and save the tag policy as a file with any name and extension in a location of your choosing\. Tag policies can have a maximum of 2,500 characters, including spaces\. 

**To create a tag policy**
+ Create a tag policy like the following:

  ```
  {
      "tags": {
          "CostCenter": {
              "tag_key": {
                  "@@assign": "CostCenter"
              }
          }
      }
  }
  ```

  This tag policy defines the `CostCenter` tag key\. The tag can accept any value or no value\. That's because when you don't specify at least one value, any value or no value is compliant\. 

## Attach a tag policy<a name="tag-policy-attach-first-cli"></a>

After you create a tag policy, you're ready to attach it to your organization root, an OU, or to an individual account\. Attaching a tag policy to the organization root affects all of your organization's member accounts\. When you attach a tag policy to an individual account, only that account is subject to that tag policy\. It is also subject to any tag policy that is attached to the organization root\.

The following procedure shows how to attach the tag policy you just created to a single test account\.
+ Attach the tag policy to your test account by running a command like the following:

  ```
  aws --region us-east-1 organizations attach-policy --target-id <account-id> --policy file://<path-and-filename>
  ```

## Determining the effective policy for an account<a name="tag-policy-get-effective-cli"></a>

To start checking compliance status for tagged resources in an account, it's helpful to first determine the effective tag policy for the account\.

**Note**  
If you did not specify a default Region when you configured the AWS CLI, you must specify the Region for account\-level commands\. You must also specify a Region if you want the command to apply to a Region other than your default\. For more information, see [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *[AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)*\.

To determine what tagging rules are attached to an account, run the following from the account and save the results to a file:

```
aws --region region-name organizations describe-effective-policy
```

If a tag policy is attached to the account as well as to the organization root, the combination of both policies defines the account's effective tag policy\. In these cases, running `describe-effective-policy` from the account returns the contents of both tag policies\. 

## Finding noncompliant resources for an account<a name="tag-policy-find-noncompliant-cli"></a>

For each account, you can get information about noncompliant resources\. You should run this command from every Region in which the account has resources\.

**Note**  
If you did not specify a default Region when you configured the AWS CLI, you must specify the Region for account\-level commands\. You must also specify a Region if you want the command to apply to a Region other than your default\. For more information, see [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *[AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)*\.

To find noncompliant resources for an account that uses the effective policy, run the following when signed in to the account and save the results to a file:

```
aws --region region-name resourcegroupstaggingapi get-resources --include-compliance-details --exclude-compliant-resources
```

Here is an example:

```
aws --region us-east-1 resourcegroupstaggingapi get-resources --include-compliance-details --exclude-compliant-resources
```

## Correcting noncompliant tags in resources<a name="tag-policy-corrections-cli"></a>

After finding noncompliant tags, make corrections using any of the following methods\. You must be signed in to the account that has the resource with noncompliant tags:
+ Use the console or API operations of the service that has the noncompliant resources\.

  Use [TagResources](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/API_TagResources.html) to add tags that are compliant with the effective policy\.
+ Use [UntagResources](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/API_UntagResources.html) to remove tags that are noncompliant with the effective tag policy\.

## Finding and correcting additional noncompliance issues<a name="tag-policy-repeat-cli"></a>

Finding and correcting compliance issues is an iterative process\. 

**To continue finding and correcting compliance issues**

1. After finding and correcting noncompliant tags in resources, rerun the `get-resources` command with the `--include-compliance-details` and `--exclude-compliant-resources` parameters to ensure that you corrected all the previously returned issues\.

   Run this command in all Regions where you have resources\. 

1. Make additional corrections\.

1. Repeat the process of finding and correcting compliance issues until the resources you care about are compliant with your tag policy\.

## Generating an organization\-wide compliance report<a name="generate-org-wide-report-cli"></a>

At any time, you can generate a report that lists all tagged resources in accounts across your organization\. The report shows whether each resource is compliant with the effective tag policy\. Note that it can take up to 48 hours for changes you make to a tag policy or resources to be reflected in the organization\-wide compliance report\. For example, assume that you have a tag policy that defines a new standardized tag for a resource type\. Resources of that type that don't have this tag are shown as compliant in the report for up to 48 hours\. 

You can generate the report from your organization's master account in the `us-east-1` Region, provided that it has access to an Amazon S3 bucket\. The bucket must have an attached bucket policy as shown in [Amazon S3 Bucket Policy for Storing Report](https://docs.aws.amazon.com/ARG/latest/userguide/tag-policies-prereqs.html#bucket-policy)\. To generate the report, run the following command:

```
aws resourcegroupstaggingapi get-compliance-summary
```

You can generate one report at a time\. 

This report can take some time to complete\. You can check the status by running the following command:

```
aws resourcegroupstaggingapi describe-report-creation
```

When the above command returns `SUCCEEDED`, you can open the report from the Amazon S3 bucket\. 