# Example SCPs for Amazon Elastic Compute Cloud \(Amazon EC2\)<a name="orgs_manage_policies_scps_examples_ec2"></a>

****Examples in this category****
+ [Require Amazon EC2 instances to use a specific type](#example-ec2-1)

## Require Amazon EC2 instances to use a specific type<a name="example-ec2-1"></a>

With this SCP, any instance launches not using the `t2.micro` instance type are denied\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "RequireMicroInstanceType",
      "Effect": "Deny",
      "Action": "ec2:RunInstances",
      "Resource": [
        "arn:aws:ec2:*:*:instance/*"
      ],
      "Condition": {
        "StringNotEquals": {
          "ec2:InstanceType": "t2.micro"
        }
      }
    }
  ]
}
```