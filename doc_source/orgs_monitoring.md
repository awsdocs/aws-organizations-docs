# Monitor the Activity in Your Organization<a name="orgs_monitoring"></a>

As a best practice, you should monitor your organization to ensure that changes are logged\. This helps you to ensure that any unexpected change can be investigated and unwanted changes can be rolled back\. AWS Organizations currently supports two AWS services that enable you to monitor your organization and the activity that happens within it\.

**Topics**
+ [AWS CloudTrail](#orgs_cloudtrail-integration)
+ [Amazon CloudWatch Events](#orgs_cloudwatch-integration)

## AWS CloudTrail<a name="orgs_cloudtrail-integration"></a>

AWS Organizations is integrated with AWS CloudTrail, a service that captures AWS Organizations API calls and delivers the log files to an Amazon S3 bucket that you specify\. CloudTrail captures API calls from the AWS Organizations console or from your code\. Using the information collected by CloudTrail, you can determine the request that was made to AWS Organizations, the source IP address from which the request was made, who made the request, when it was made, and so on\. 

AWS Organizations is also integrated with the **Event history** feature in CloudTrail\. If an API for Organizations is supported in **Event history**, you can view the most recent 90 days of events in Organizations in the CloudTrail console in **Event history** even if you have not configured any logs in CloudTrail\.

**Important**  
You can view all CloudTrail information for AWS Organizations only in the US East \(N\. Virginia\) region\. If you don't see your Organizations activity in the CloudTrail console, set your console to **US East \(N\. Virginia\)** using the menu in the upper\-right corner\. If you query CloudTrail with the CLI or SDK tools, direct your query to the US East \(N\. Virginia\) endpoint\.

To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

### AWS Organizations Information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

CloudTrail is enabled on your AWS account when you create the account\. When activity occurs in Organizations, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download the past 90 days of supported activity in your AWS account\. For more information, see [Viewing Events with CloudTrail Event History](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html) and [Services Supported by CloudTrail Event History](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events-supported-services.html)\.

When CloudTrail logging is enabled in your AWS account, API calls made to AWS Organizations actions are tracked in CloudTrail log files, where they are written with other AWS service records\. CloudTrail determines when to create and write to a new file based on a time period and file size\.

All AWS Organizations actions are logged by CloudTrail and are documented in the [AWS Organizations API Reference](http://docs.aws.amazon.com/organizations/latest/APIReference/)\. For example, calls to the `ListHandshakesForAccount`, `CreatePolicy`, and `InviteAccountToOrganization` operations generate entries in the CloudTrail log files\. 

Every log entry contains information about who generated the request\. The user identity information in the log entry helps you determine the following: 
+ Whether the request was made with root or IAM user credentials
+ Whether the request was made with temporary security credentials for an [IAM role](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) or a [federated user](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers.html) whose security credentials are validated by an external identity provider instead of directly by AWS
+ Whether the request was made by another AWS service

For more information, see the [CloudTrail userIdentity Element](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

You can store your log files in your Amazon S3 bucket for as long as you want, but you can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, your log files are encrypted with Amazon S3 server\-side encryption \(SSE\)\.

If you want to be notified upon log file delivery, you can configure CloudTrail to publish Amazon SNS notifications when new log files are delivered\. For more information, see [Configuring Amazon SNS Notifications for CloudTrail](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)\.

### Understanding AWS Organizations Log File Entries<a name="understanding-service-name-entries"></a>

CloudTrail log files can contain one or more log entries\. Each entry lists multiple JSON\-formatted events\. A log entry represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. Log entries are not an ordered stack trace of the public API calls, so they do not appear in any specific order\. 

The following example shows a CloudTrail log entry for a sample `CreateAccount` call:

```
{
    "eventVersion": "1.04",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "AIDAMVNPBQA3EXAMPLE",
        "arn": "arn:aws:iam::111122223333:user/bob",
        "accountId": "111122223333",
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "userName": "bob"
    },
    "eventTime": "2017-01-18T21:39:20Z",
    "eventSource": "organizations.amazonaws.com",
    "eventName": "CreateAccount",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "192.168.0.1",
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.95 Safari/537.36",
    "requestParameters": {
        "email": "anika@amazon.com",
        "accountName": "ProductionAccount"
    },
    "responseElements": {
        "createAccountStatus": {
            "accountName": "ProductionAccount",
            "state": "IN_PROGRESS",
            "id": "car-examplecreateaccountrequestid111",
            "requestedTimestamp": "Jan 18, 2017 9:39:19 PM"
        }
    },
    "requestID": "EXAMPLE8-90ab-cdef-fedc-ba987EXAMPLE",
    "eventID": "EXAMPLE8-90ab-cdef-fedc-ba987EXAMPLE",
    "eventType": "AwsApiCall",
    "recipientAccountId": "111111111111"
}
```

The following example shows a CloudTrail log entry for a sample `CreateOrganizationalUnit` call:

```
{
    "eventVersion": "1.04",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "AIDAMVNPBQA3EXAMPLE",
        "arn": "arn:aws:iam::111111111111:user/bob",
        "accountId": "111111111111",
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "userName": "bob"
    },
    "eventTime": "2017-01-18T21:40:11Z",
    "eventSource": "organizations.amazonaws.com",
    "eventName": "CreateOrganizationalUnit",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "192.168.0.1",
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.95 Safari/537.36",
    "requestParameters": {
        "name": "OU-Developers-1",
        "parentId": "r-examplerootid111"
    },
    "responseElements": {
        "organizationalUnit": {
            "arn": "arn:aws:organizations::111111111111:ou/o-exampleorgid/ou-examplerootid111-exampleouid111",
            "id": "ou-examplerootid111-exampleouid111",
            "name": "test-cloud-trail"
        }
    },
    "requestID": "EXAMPLE8-90ab-cdef-fedc-ba987EXAMPLE",
    "eventID": "EXAMPLE8-90ab-cdef-fedc-ba987EXAMPLE",
    "eventType": "AwsApiCall",
    "recipientAccountId": "111111111111"
}
```

The following example shows a CloudTrail log entry for a sample `InviteAccountToOrganization` call:

```
{
    "eventVersion": "1.04",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "AIDAMVNPBQA3EXAMPLE",
        "arn": "arn:aws:iam::111111111111:user/bob",
        "accountId": "111111111111",
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "userName": "bob"
    },
    "eventTime": "2017-01-18T21:41:17Z",
    "eventSource": "organizations.amazonaws.com",
    "eventName": "InviteAccountToOrganization",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "192.168.0.1",
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.95 Safari/537.36",
    "requestParameters": {
        "notes": "This is a request for Alice's account to join Bob's organization.",
        "target": {
            "type": "ACCOUNT",
            "id": "222222222222"
        }
    },
    "responseElements": {
        "handshake": {
            "requestedTimestamp": "Jan 18, 2017 9:41:16 PM",
            "state": "OPEN",
            "arn": "arn:aws:organizations::111111111111:handshake/o-exampleorgid/invite/h-examplehandshakeid111",
            "id": "h-examplehandshakeid111",
            "parties": [
                {
                    "type": "ORGANIZATION",
                    "id": "o-exampleorgid"
                },
                {
                    "type": "ACCOUNT",
                    "id": "222222222222"
                }
            ],
            "action": "invite",
            "expirationTimestamp": "Feb 2, 2017 9:41:16 PM",
            "resources": [
                {
                    "resources": [
                        {
                            "type": "MASTER_EMAIL",
                            "value": "bob@example.com"
                        },
                        {
                            "type": "MASTER_NAME",
                            "value": "Master account for organization"
                        },
                        {
                            "type": "ORGANIZATION_FEATURE_SET",
                            "value": "ALL"
                        }
                    ],
                    "type": "ORGANIZATION",
                    "value": "o-exampleorgid"
                },
                {
                    "type": "ACCOUNT",
                    "value": "222222222222"
                },
                {
                    "type": "NOTES",
                    "value": "This is a request for Alice's account to join Bob's organization."
                }
            ]
        }
    },
    "requestID": "EXAMPLE8-90ab-cdef-fedc-ba987EXAMPLE",
    "eventID": "EXAMPLE8-90ab-cdef-fedc-ba987EXAMPLE",
    "eventType": "AwsApiCall",
    "recipientAccountId": "111111111111"
}
```

The following example shows a CloudTrail log entry for a sample `AttachPolicy` call\. The response indicates that the call failed because the requested policy type is not enabled in the root in which the request to attach was attempted:

```
{
    "eventVersion": "1.04",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "AIDAMVNPBQA3EXAMPLE",
        "arn": "arn:aws:iam::111111111111:user/bob",
        "accountId": "111111111111",
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "userName": "bob"
    },
    "eventTime": "2017-01-18T21:42:44Z",
    "eventSource": "organizations.amazonaws.com",
    "eventName": "AttachPolicy",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "192.168.0.1",
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.95 Safari/537.36",
    "errorCode": "PolicyTypeNotEnabledException",
    "errorMessage": "The given policy type ServiceControlPolicy is not enabled on the current view",
    "requestParameters": {
        "policyId": "p-examplepolicyid111",
        "targetId": "ou-examplerootid111-exampleouid111"
    },
    "responseElements": null,
    "requestID": "EXAMPLE8-90ab-cdef-fedc-ba987EXAMPLE",
    "eventID": "EXAMPLE8-90ab-cdef-fedc-ba987EXAMPLE",
    "eventType": "AwsApiCall",
    "recipientAccountId": "111111111111"
}
```

## Amazon CloudWatch Events<a name="orgs_cloudwatch-integration"></a>

AWS Organizations can work with CloudWatch Events to raise "events" when administrator\-specified actions occur in an organization\. For example, because of the sensitivity of such actions, most administrators would want to be warned every time someone creates a new account in the organization, or when an administrator of a member account attempts to leave the organization\. You can configure CloudWatch Events rules that look for these actions and then send the generated events to administrator defined "targets"\. Targets can be an Amazon SNS topic that emails or text messages its subscribers\. You could also create a simple AWS Lambda function that logs the details of the action for your later review\.

For a tutorial that shows how to enable CloudWatch Events to monitor key activity in your organization, see [Tutorial: Monitor Important Changes to Your Organization with CloudWatch Events ](orgs_tutorials_cwe.md)\.

To learn more about CloudWatch Events, inclucing how to configure and enable it, see the [Amazon CloudWatch Events User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/)\.