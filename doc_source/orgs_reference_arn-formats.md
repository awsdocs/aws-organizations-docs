# ARN Formats Supported by AWS Organizations<a name="orgs_reference_arn-formats"></a>

The following table shows the ARN formats that are supported in IAM policies for AWS Organizations\. You can view the IDs for each entity in the details pane on the right side of the AWS Organizations console\. If you are already familiar with ARNs, you will notice a slight difference in how they are handled compared to other services\. The **<OrganizationId>** field is where most services place an AWS account ID\. This is because AWS Organizations spans multiple accounts\.

If you see an expand arrow \(**↗** \) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


****  

| Resource Type | ARN Format | 
| --- | --- | 
| <a name="arn_organization"></a>Organization | arn:aws:organizations::<masterAccountId>:organization/o\-<organizationId> | 
| <a name="arn_root"></a>Root | arn:aws:organizations::<masterAccountId>:root/o\-<organizationId>/r\-<rootId> | 
| <a name="arn_account"></a>Account | arn:aws:organizations::<masterAccountId>:account/o\-<organizationId>/<accountId> | 
| <a name="arn_ou"></a>Organizational Unit | arn:aws:organizations::<masterAccountId>:ou/o\-<organizationId>/ou\-<organizationalUnitId> | 
| <a name="arn_policy"></a>Policy | arn:aws:organizations::<masterAccountId>:policy/o\-<organizationId>/<policyType>/p\-<policyId> | 
| <a name="arn_handshake"></a>Handshake | arn:aws:organizations::<masterAccountId>:handshake/o\-<organizationId>/<handshakeType>/h\-<handshakeId> | 