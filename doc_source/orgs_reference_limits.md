# Limits of AWS Organizations<a name="orgs_reference_limits"></a>

This section specifies limits that affect AWS Organizations\.

## Limits on Names<a name="name-limits"></a>

The following are restrictions on names that you create in AWS Organizations, including names of accounts, organizational units \(OUs\), roots, and policies:
+ They must be composed of Unicode characters
+ They must not exceed 250 characters in length

## Maximum and Minimum Values<a name="min-max-values"></a>

The following are the default maximums for entities in AWS Organizations\.


****  

|  |  | 
| --- |--- |
|  Number of AWS accounts in an organization  |  Varies\. *If you need to increase your limit, contact AWS Support\. In the upper\-right corner of the console, choose **Support** and then **Support Center**\. On the **Support Center** page, choose **Create case**\.* An invitation sent to an account counts against this limit\. The count is returned if the invited account declines, the master account cancels the invitation, or the invitation expires\.  | 
|  Number of roots in an organization  |  1\.  | 
|  Number of OUs in an organization  |  1,000\.  | 
|  Number of policies in an organization  |  1,000\.  | 
|  Maximum size of a [service control policy \(SCP\)](orgs_manage_policies_scp.md) document  |  5,120 bytes\. This includes all characters, including white space\. To reduce the size of your SCP if you approach the limit, you can remove all white space characters \(such as spaces and line breaks\) that are outside quotation marks\.  | 
|  OU maximum nesting in a root  |  Five levels of OUs deep under a root\.  | 
|  Number of open invitations you can add in a 24\-hour period  |  20 — Accepted invitations don't count against this limit\. As soon as one invitation is accepted, you can send another invitation that same day\.  | 
|  Number of member accounts you can create concurrently  |  3 — As soon as one finishes, you can start another, but only three can be in progress at a time\.  | 
|  Number of entities that you can attach a policy to  |  Unlimited\.  | 
|  Number of tags that you can attach to an account  |  50\.  | 

**Expiration times for handshakes**

The following are the timeouts for handshakes in AWS Organizations\.


****  

|  |  | 
| --- |--- |
|  Invitation to join an organization  | 15 days | 
| Request to enable all features in an organization | 90 days | 
| Handshake is deleted and no longer appears in lists | 30 days after the handshake is completed | 

**Number of policies that you can attach to an entity**

The maximum depends on the policy type and the entity that you're attaching the policy to\. The following table shows each policy type and the number of entities that you can attach each type to\.


****  

| Policy Type | Policies per Root | Policies per OU | Policies per Account | 
| --- | --- | --- | --- | 
| Service control policy | 5 | 5 | 5 | 

**Note**  
Currently, you can have only one root in an organization\.

The minimum depends on the policy type\. The following table shows each policy type and the minimum number of entities that you can attach each type to\.


****  

| Policy Type | Minimum Allowed Attached to an Entity | 
| --- | --- | 
| Service control policy | 1 — Every entity must have at least one SCP attached at all times\. You can't remove the last SCP from an entity\. | 