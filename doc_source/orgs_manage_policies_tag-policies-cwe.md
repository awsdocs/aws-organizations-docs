# Using CloudWatch Events to monitor noncompliant tags<a name="orgs_manage_policies_tag-policies-cwe"></a>

You can use CloudWatch Events to monitor when noncompliant tags are introduced\. In the following example event, the `"false"` value for `tag-policy-compliant` indicates that a new tag is noncompliant with the effective tag policy\.

```
{
  "detail-type": "Tag Change on Resource",
  "region": "us-east-1",
  "resources": [
    "arn:aws:ec2:us-east-1:123456789012:instance/i-0000000aaaaaaaaaa"
  ],
  "detail": {
    "changed-tag-keys": [
      "a-new-key"
    ],
    "service": "ec2",
    "resource-type": "instance",
    "version": 3,
    "tag-policy-compliant": "false",
    "tags": {
      "a-new-key": "tag-value-on-new-key-just-added"
    }
  }
}
```

You can subscribe to events and specify strings or patterns to monitor\. For more information on CloudWatch Events, see the *[Amazon CloudWatch Events User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/)*\.