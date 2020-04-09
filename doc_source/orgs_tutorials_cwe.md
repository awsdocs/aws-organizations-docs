# Tutorial: Monitor important changes to your organization with CloudWatch Events<a name="orgs_tutorials_cwe"></a>

This tutorial shows how to configure CloudWatch Events to monitor your organization for changes\. You start by configuring a rule that is triggered when users invoke specific AWS Organizations operations\. Next, you configure CloudWatch Events to run an AWS Lambda function when the rule is triggered, and you configure Amazon SNS to send an email with details about the event\. 

The following illustration shows the main steps of the tutorial\.



**[Step 1: Configure a trail and event selector](#tutorial-cwe-step1)**  
Create a log, called a *trail*, in AWS CloudTrail\. You configure it to capture all API calls\.

**[Step 2: Configure a Lambda function ](#tutorial-cwe-step2)**  
Create an AWS Lambda function that logs details about the event to an S3 bucket\.

**[Step 3: Create an Amazon SNS topic that sends emails to subscribers](#tutorial-cwe-step3)**  
Create an Amazon SNS topic that sends emails to its subscribers, and then subscribe yourself to the topic\.

**[Step 4: Create a CloudWatch Events rule](#tutorial-cwe-step4)**  
Create a rule that tells CloudWatch Events to pass details of specified API calls to the Lambda function and to SNS topic subscribers\.

**[Step 5: Test your CloudWatch Events rule](#tutorial-cwe-step5)**  
Test your new rule by running one of the monitored operations\. In this tutorial, the monitored operation is creating an organizational unit \(OU\)\. You view the log entry that the Lambda function creates, and you view the email that Amazon SNS sends to subscribers\.

**Tip**  
You can also use this tutorial as a guide in configuring similar operations, such as sending email notifications when account creation is complete\. Because account creation is an asynchronous operation, you're not notified by default when it completes\. For more information on using AWS CloudTrail and CloudWatch Events with AWS Organizations, see [Logging and monitoring in AWS Organizations](orgs_security_incident-response.md)\.

## Prerequisites<a name="tutorial-cwe-prereqs"></a>

This tutorial assumes the following:
+ You can sign in to the AWS Management Console as an IAM user from the master account in your organization\. The IAM user must have permissions to create and configure a log in CloudTrail, a function in Lambda, a topic in Amazon SNS, and a rule in CloudWatch\. For more information about granting permissions, see [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*, or the guide for the service for which you want to configure access\.
+ You have access to an existing Amazon Simple Storage Service \(Amazon S3\) bucket \(or you have permissions to create a bucket\) to receive the CloudTrail log that you configure in step 1\.

**Important**  
Currently, AWS Organizations is hosted in only the US East \(N\. Virginia\) Region \(even though it is available globally\)\. To perform the steps in this tutorial, you must configure the AWS Management Console to use that region\. 

## Step 1: Configure a trail and event selector<a name="tutorial-cwe-step1"></a>

In this step, you sign in to the master account and configure a log \(called a *trail*\) in AWS CloudTrail\. You also configure an event selector on the trail to capture all read/write API calls so that CloudWatch Events has calls to trigger on\.

**To create a trail**

1. Sign in to AWS as an administrator of the organization's master account and then open the CloudTrail console at [https://console.aws.amazon.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. On the navigation bar in the upper\-right corner of the console, choose the **US East \(N\. Virginia\)** Region\. If you choose a different region, AWS Organizations doesn't appear as an option in the CloudWatch Events configuration settings, and CloudTrail doesn't capture information about AWS Organizations\.

1. In the navigation pane, choose **Trails**\.

1. Choose **Create trail**\.

1. For **Trail name**, enter **My\-Test\-Trail**\. 

1. Perform one of the following options to specify where CloudTrail is to deliver its logs:
   + If you already have a bucket, choose **No** next to **Create a new S3 bucket** and then choose the bucket name from the **S3 bucket** list\.
   + If you need to create a bucket, choose **Yes** next to **Create a new S3 bucket** and then, for **S3 bucket**, enter a name for the new bucket\.
**Note**  
S3 bucket names must be ***globally*** unique\.

1. Choose **Create**\.

1. Choose the trail `My-Test-Trail` that you just created\.

1. Choose the pencil icon next to **Management events**\.

1. For **Read/Write events**, choose **All**, choose **Save**, and then choose **Configure**\.

CloudWatch Events enables you to choose from several different ways to send alerts when an alarm rule matches an incoming API call\. This tutorial demonstrates two methods: invoking a Lambda function that can log the API call and sending information to an Amazon SNS topic that sends an email or text message to the topic's subscribers\. In the next two steps, you create the components you need: the Lambda function, and the Amazon SNS topic\.

## Step 2: Configure a Lambda function<a name="tutorial-cwe-step2"></a>

In this step, you create a Lambda function that logs the API activity that is sent to it by the CloudWatch Events rule that you configure later\.

**To create a Lambda function that logs CloudWatch Events events**

1. Open the AWS Lambda console at [https://console.aws.amazon.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. If you are new to Lambda, choose **Get Started Now** on the welcome page; otherwise, choose **Create a function**\.

1. On the **Create function** page, choose **Blueprints**\.

1. From the **Blueprints** search box, enter **hello** for the filter and choose the **hello\-world** blueprint\.

1. Choose **Configure**\.

1. On the **Basic information** page, do the following:

   1. For the Lambda function name, enter **LogOrganizationEvents** in the **Name** text box\. 

   1. For **Role**, choose **Create a custom role** and then, at the bottom of the **AWS Lambda requires access to your resources** page, choose **Allow**\. This role grants your Lambda function permissions to access the data it requires and to write its output log\.

   1. Choose **Create function**\. 

1. On the next page, edit the code for the Lambda function, as shown in the following example\.

   ```
   console.log('Loading function');
   
   exports.handler = async (event, context) => {
       console.log('LogOrganizationsEvents');
       console.log('Received event:', JSON.stringify(event, null, 2));
       return event.key1;  // Echo back the first key value
       // throw new Error('Something went wrong');
   };
   ```

   This sample code logs the event with a **LogOrganizationEvents** marker string followed by the JSON string that makes up the event\.

1. Choose **Save**\.

## Step 3: Create an Amazon SNS topic that sends emails to subscribers<a name="tutorial-cwe-step3"></a>

In this step, you create an Amazon SNS topic that emails information to its subscribers\. You make this topic a "target" of the CloudWatch Events rule that you create later\.

**To create an Amazon SNS topic to send an email to subscribers**

1. Open the Amazon SNS console at [https://console.aws.amazon.com/sns/v3/](https://console.aws.amazon.com/sns/v3/)\. 

1. In the navigation pane, choose **Topics**\.

1. Choose **Create new topic**\.

   1. For **Topic name**, enter **OrganizationsCloudWatchTopic**\.

   1. For **Display name**, enter **OrgsCWEvnt**\.

   1. Choose **Create topic**\.

1. Now you can create a subscription for the topic\. Choose the ARN for the topic that you just created\.

1. Choose **Create subscription**\.

   1. On the **Create subscription** page, for **Protocol**, choose **Email**\.

   1. For **Endpoint**, enter your email address\.

   1. Choose **Create subscription**\. AWS sends an email to the email address that you specified in the preceding step\. Wait for that email to arrive, and then choose the **Confirm subscription** link in the email to verify that you successfully received the email\.

   1. Return to the console and refresh the page\. The **Pending confirmation** message disappears and is replaced by the now valid subscription ID\.

## Step 4: Create a CloudWatch Events rule<a name="tutorial-cwe-step4"></a>

Now that the required Lambda function exists in your account, you create a CloudWatch Events rule that invokes it when the criteria in the rule are met\.

**To create a CloudWatch Events rule**

1. Open the CloudWatch console at [https://console.aws.amazon.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Rules** and then choose **Create rule**\.

1. For **Event source**, do the following:

   1. Choose **Event pattern**\.

   1. Choose **Build event pattern to match events by service**\.

   1. For **Service Name**, choose **Organizations**\.

   1. For **Event Type**, choose **AWS API Call via CloudTrail**\.

   1. Choose **Specific operation\(s\)** and then enter the APIs that you want monitored: **CreateAccount** and **CreateOrganizationalUnit**\. You can select any others that you also want\. For a complete list of available AWS Organizations APIs, see the [AWS Organizations API Reference](https://docs.aws.amazon.com/organizations/latest/APIReference/)\.

1. Under **Targets**, for **Function**, choose the function that you created in the previous procedure\.

1. Under **Targets**, choose **Add target**\.

1. In the new target row, choose the dropdown header and then choose **SNS topic**\.

1. For **Topic**, choose the topic named **OrganizationCloudWatchTopic** that you created in the preceding procedure\.

1. Choose **Configure details**\.

1. On the **Configure rule details** page, for **Name** enter **OrgsMonitorRule**, leave **State** selected and then choose **Create rule**\.

## Step 5: Test your CloudWatch Events rule<a name="tutorial-cwe-step5"></a>

In this step, you create an organizational unit \(OU\) and observe the CloudWatch Events rule generate a log entry and send an email to you with details about the event\.

**To create an OU**

1. Open the AWS Organizations console at [https://console.aws.amazon.com/organizations/](https://console.aws.amazon.com/organizations/)\. 

1. Choose the **Organize accounts** tab and then choose **New organizational unit**\.

1. For the name of the OU, enter **TestCWEOU** and then choose **Create organizational unit**\.

**To see the CloudWatch Events log entry**

1. Open the CloudWatch console at [https://console.aws.amazon.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation page, choose **Logs**\.

1. Under **Log Groups**, choose the group that is associated with your Lambda function: **/aws/lambda/LogOrganizationEvents**\.

1. Each group contains one or more streams, and there should be one group for today\. Choose it\.

1. View the log\. You should see rows similar to the following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/tutorial-sample-CWE-log.png)

1. Select the middle row of the entry to see the full JSON text of the received event\. You can see all the details of the API request in the `requestParameters` and `responseElements` pieces of the output\.

   ```
   2017-03-09T22:45:05.101Z 0999eb20-051a-11e7-a426-cddb46425f16 Received event:
   {
       "version": "0",
       "id": "123456-EXAMPLE-GUID-123456",
       "detail-type": "AWS API Call via CloudTrail",
       "source": "aws.organizations",
       "account": "123456789012",
       "time": "2017-03-09T22:44:26Z",
       "region": "us-east-1",
       "resources": [],
       "detail": {
           "eventVersion": "1.04",
           "userIdentity": {
               ...
           },
           "eventTime": "2017-03-09T22:44:26Z",
           "eventSource": "organizations.amazonaws.com",
           "eventName": "CreateOrganizationalUnit",
           "awsRegion": "us-east-1",
           "sourceIPAddress": "192.168.0.1",
           "userAgent": "AWS Organizations Console, aws-internal/3",
           "requestParameters": {
               "parentId": "r-exampleRootId",
               "name": "TestCWEOU"
           },
           "responseElements": {
               "organizationalUnit": {
                   "name": "TestCWEOU",
                   "id": "ou-exampleRootId-exampleOUId",
                   "arn": "arn:aws:organizations::1234567789012:ou/o-exampleOrgId/ou-exampleRootId-exampeOUId"
               }
           },
           "requestID": "123456-EXAMPLE-GUID-123456",
           "eventID": "123456-EXAMPLE-GUID-123456",
           "eventType": "AwsApiCall"
       }
   }
   ```

1. Check your email account for a message from **OrgsCWEvnt** \(the display name of your Amazon SNS topic\)\. The body of the email contains the same JSON text output as the log entry that is shown in the preceding step\.

## Clean up: Remove the resources you no longer need<a name="clean-up-resources"></a>

To avoid incurring charges, you should delete any AWS resources that you created as part of this tutorial that you don't want to keep\.

**To clean up your AWS environment**

1. Use the CloudTrail console at [https://console.aws.amazon.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/) to delete the trail named **My\-Test\-Trail** that you created in step 1\.

1. If you created an Amazon S3 bucket in step 1, use the Amazon S3 console at [https://console.aws.amazon.com/s3/](https://console.aws.amazon.com/s3/) to delete it\.

1. Use the Lambda console at [https://console.aws.amazon.com/lambda/](https://console.aws.amazon.com/lambda/) to delete the function named **LogOrganizationEvents** that you created in step 2\.

1. Use the Amazon SNS console at [https://console.aws.amazon.com/sns/](https://console.aws.amazon.com/sns/) to delete the Amazon SNS topic named **OrganizationsCloudWatchTopic** that you created in step 3\.

1. Use the CloudWatch console at [https://console.aws.amazon.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/) to delete the CloudWatch rule named **OrgsMonitorRule** that you created in step 4\.

That's it\. In this tutorial, you configured CloudWatch Events to monitor your organization for changes\. You configured a rule that is triggered when users invoke specific AWS Organizations operations\. The rule ran a Lambda function that logged the event and sent an email that contains details about the event\.