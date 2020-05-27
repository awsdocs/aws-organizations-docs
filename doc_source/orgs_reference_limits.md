# Quotas for AWS Organizations<a name="orgs_reference_limits"></a>

This section specifies quotas that affect AWS Organizations\.

## Naming guidelines<a name="name-limits"></a>

The following are guidelines for names that you create in AWS Organizations, including names of accounts, organizational units \(OUs\), roots, and policies:
+ They must be composed of Unicode characters
+ They must not exceed 250 characters in length

## Maximum and minimum values<a name="min-max-values"></a>

The following are the default maximums for entities in AWS Organizations\.


****  

|  |  | 
| --- |--- |
|  Number of AWS accounts in an organization  |   4 is the default maximum number of accounts allowed in an organization\. *If you need to increase your quota, contact AWS Support\. In the upper\-right corner of the console, choose **Support** and then **Support Center**\. On the **Support Center** page, choose **Create case**\.* An invitation sent to an account counts against this quota\. The count is returned if the invited account declines, the master account cancels the invitation, or the invitation expires\.  | 
|  Number of roots in an organization  |  1\.  | 
|  Number of OUs in an organization  |  1,000\.  | 
|  Number of policies in an organization  |  1,000\.  | 
|  Maximum size of a [service control policy \(SCP\)](orgs_manage_policies_scp.md) document  |  5,120 bytes\. White space \(such as spaces and line breaks\) between JSON elements and outside of quotation marks, is not counted\.   | 
|  Maximum size of a [tag policy](orgs_manage_policies_tag-policies.md) document  |  2,500 characters\. White space \(such as spaces and line breaks\) between JSON elements and outside of quotation marks, is not counted\.   | 
|  OU maximum nesting in a root  |  Five levels of OUs deep under a root\.  | 
|  Number of open invitations you can add in a 24\-hour period  |  20 — Accepted invitations don't count against this quota\. As soon as one invitation is accepted, you can send another invitation that same day\.  | 
|  Number of member accounts you can create concurrently  |  5 — As soon as one finishes, you can start another, but only five can be in progress at a time\.  | 
|  Number of entities that you can attach a policy to  |  Unlimited\.  | 
|  Number of tags that you can attach to an account  |  50\.  | 

### Expiration times for handshakes<a name="min-max-handshakes"></a>

The following are the timeouts for handshakes in AWS Organizations\.


****  

|  |  | 
| --- |--- |
|  Invitation to join an organization  |  15 days  | 
|  Request to enable all features in an organization  |  90 days  | 
|  Handshake is deleted and no longer appears in lists  |  30 days after the handshake is completed  | 

### Number of policies that you can attach to an entity<a name="min-max-policies"></a>

The maximum depends on the policy type and the entity that you're attaching the policy to\. The following table shows each policy type and the number of entities that you can attach each type to\.


****  

| Policy type | Policies per root | Policies per OU | Policies per account | 
| --- | --- | --- | --- | 
| Service control policy | 5 | 5 | 5 | 
| Tag policy | 5 | 5 | 5 | 

**Note**  
Currently, you can have only one root in an organization\.

The minimum depends on the policy type\. The following table shows each policy type and the minimum number of entities that you can attach each type to\.


****  

| Policy type | Minimum allowed attached to an entity | 
| --- | --- | 
| Service control policy | 1 — Every entity must have at least one SCP attached at all times\. You can't remove the last SCP from an entity\. | 
| Tag policy | 0 | 