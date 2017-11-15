# Limits of AWS Organizations<a name="orgs_reference_limits"></a>

This section specifies limits that affect AWS Organizations\.

## Limits on Names<a name="name-limits"></a>

The following are restrictions on names that you create in AWS Organizations \(including names of accounts, OUs, roots, and policies\):

+ They must be composed of Unicode characters\.

+ They must not exceed 250 characters in length\.

## Maximum and Minimum Values<a name="min-max-values"></a>

The following are the default maximums for entities in AWS Organizations:


****  

|  |  | 
| --- |--- |
|  Number of AWS accounts in an organization  |  Varies\. Contact Customer Support if you need to increase your limit\. An invitation sent to an account counts against this limit\. The count is returned if the invited account declines, the master account cancels the invitation, or the invitation expires\.  | 
|  Number of roots in an organization  |  1  | 
| Number of OUs in an organization | 1,000 | 
| Number of policies in an organization | 1,000 | 
| Maximum size of a service control policy document | 5,120 bytes | 
| OU maximum nesting in a root | 5 levels of OUs deep under a root | 
| Invitations sent per day | 20 | 
| Number of member accounts you can create concurrently | 5 — As soon as one finishes you can start another, but only five can be in progress at a time\) | 
| Number of entities to which you can attach a policy | Unlimited | 

**Expiration times for handshakes**

The following are the timeouts for handshakes in AWS Organizations:


****  

|  |  | 
| --- |--- |
|  Invitation to join an organization  | 15 days | 
| Request to enable all features in an organization | 90 days | 
| Handshake is completed | 30 days before the handshake is deleted and no longer appears in lists\. | 

**Number of policies that can be attached to an entity**

The maximum depends on the policy type as well as the entity that you're attaching the policy to\. The following table shows each policy type and the number of entities to which each can be attached:


****  

| Policy type | Policies per root | Policies per OU | Policies per account | 
| --- | --- | --- | --- | 
| Service control policy | 5 | 5 | 5 | 

**Note**  
Currently, you can have only one root in an organization\.

The minimum depends on the policy type\. The following table shows each policy type and the minimum number of entities to which each can be attached:


****  

| Policy type | Minimum allowed attached to an entity | 
| --- | --- | 
| Service control policy | 1 \- Every entity must have at least one SCP attached at all times\. You can't remove the last SCP from an entity\. | 