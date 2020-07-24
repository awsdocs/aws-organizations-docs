# AWS Compute Optimizer and AWS Organizations<a name="services-that-can-integrate-compute-optimizer"></a>

AWS Compute Optimizer is a service that analyzes the configuration and utilization metrics of your AWS resources\. Resource examples include Amazon Elastic Compute Cloud \(Amazon EC2\) instances and Auto Scaling groups\. Compute Optimizer reports whether your resources are optimal and generates optimization recommendations to reduce the cost and improve the performance of your workloads\. For more information about Compute Optimizer, see the [AWS Compute Optimizer User Guide](https://docs.aws.amazon.com/compute-optimizer/latest/ug/what-is.html)\.

The following list provides information that is useful to know when you want to integrate AWS Compute Optimizer and AWS Organizations:
+ **To enable trusted access with AWS Organizations** – You must sign in with your AWS Organizations master account and opt in your accounts\. For information, see [Opting in your Account](https://docs.aws.amazon.com/compute-optimizer/latest/ug/getting-started.html#account-opt-in) in the *AWS Compute Optimizer User Guide*\.
+ **Service principal name for AWS Compute Optimizer: ** `compute-optimizer.amazonaws.com`
+ **Name of the IAM service\-linked role that can be created in accounts when trusted access is enabled: ** `AWSServiceRoleForComputeOptimizer`