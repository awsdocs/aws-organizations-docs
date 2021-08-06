# Example SCPs for Amazon Virtual Private Cloud \(Amazon VPC\)<a name="orgs_manage_policies_scps_examples_vpc"></a>

****Examples in this category****
+ [Prevent users from deleting Amazon VPC flow logs](#example_vpc_1)
+ [Prevent any VPC that doesn't already have internet access from getting it](#example_vpc_2)

## Prevent users from deleting Amazon VPC flow logs<a name="example_vpc_1"></a>

This SCP prevents users or roles in any affected account from deleting Amazon Elastic Compute Cloud \(Amazon EC2\) flow logs or CloudWatch log groups or log streams\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "ec2:DeleteFlowLogs",
        "logs:DeleteLogGroup",
        "logs:DeleteLogStream"
      ],
      "Resource": "*"
    }
  ]
 }
```

## Prevent any VPC that doesn't already have internet access from getting it<a name="example_vpc_2"></a>

This SCP prevents users or roles in any affected account from changing the configuration of your Amazon EC2 virtual private clouds \(VPCs\) to grant them direct access to the internet\. It doesn't block existing direct access or any access that routes through your on\-premises network environment\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "ec2:AttachInternetGateway",
        "ec2:CreateInternetGateway",
        "ec2:CreateEgressOnlyInternetGateway",
        "ec2:CreateVpcPeeringConnection",
        "ec2:AcceptVpcPeeringConnection",
        "globalaccelerator:Create*",
        "globalaccelerator:Update*"
      ],
      "Resource": "*"
    }
  ]
}
```