# Quotas for AWS Organizations<a name="orgs_reference_limits"></a>

This section specifies quotas that affect AWS Organizations\.

## Naming guidelines<a name="name-limits"></a>

The following are guidelines for names that you create in AWS Organizations, including names of accounts, organizational units \(OUs\), roots, and policies:
+ They must be composed of Unicode characters
+ Maximum string length for names vary by the object\. To see actual limit for each, see the [AWS Organizations API Reference](https://docs.aws.amazon.com/organizations/latest/APIReference/) and find the API operation that creates the object\. Look at the details for that operation's `Name` parameter\. For example: [Account name](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreateAccount.html#organizations-CreateAccount-request-AccountName), or [OU name](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreateOrganizationalUnit.html#organizations-CreateOrganizationalUnit-request-Name)\.

## Maximum and minimum values<a name="min-max-values"></a>

The following are the ***default*** maximums for entities in AWS Organizations\. 

**Note**  
You can request increases for *some* of these values by using the [Service Quotas console](https://console.aws.amazon.com/servicequotas/home?region=us-east-1#!/services/organizations/quotas)\.   
Organizations is a global service that is physically hosted in the US East \(N\. Virginia\) Region \(`us-east-1`\)\. Therefore, you must use `us-east-1` to access Organizations quotas when using the Service Quotas console, the AWS CLI, or an AWS SDK\.


****  

|  |  | 
| --- |--- |
|  Number of AWS accounts in an organization  |   4  This is the default maximum number of accounts allowed in an organization\.  An invitation sent to an account counts against this quota\. The count is returned if the invited account declines, the master account cancels the invitation, or the invitation expires\.  | 
|  Number of roots in an organization  |  1  | 
|  Number of OUs in an organization  |  1000  | 
|  Number of policies of each type in an organization  |  1000 per policy type  | 
|  Maximum size of a policy document  |  Service control policies: 5120 bytes *\(not characters\)* AI services opt\-out policies: 2500 characters Backup policies: 10,000 characters Tag policies: 2500 characters **Note:** If you save the policy by using the AWS Management Console, extra white space \(such as spaces and line breaks\) between JSON elements and outside of quotation marks, is removed and not counted\. If you save the policy using an SDK operation or the AWS CLI, then the policy is saved exactly as you provided and no automatic removal of characters occurs\.   | 
|  OU maximum nesting in a root  |  Five levels of OUs deep under a root\.  | 
|  Number of open invitations you can add in a 24\-hour period  |  20 — Accepted invitations don't count against this quota\. As soon as one invitation is accepted, you can send another invitation that same day\.  | 
|  Number of member accounts you can create concurrently  |  5 — As soon as one finishes, you can start another, but only five can be in progress at a time\.  | 
|  Number of entities that you can attach a policy to  |  Unlimited  | 
|  Number of tags that you can attach to an account  |  50  | 

### Expiration times for handshakes<a name="min-max-handshakes"></a>

The following are the timeouts for handshakes in AWS Organizations\.


****  

|  |  | 
| --- |--- |
|  Invitation to join an organization  |  15 days  | 
|  Request to enable all features in an organization  |  90 days  | 
|  Handshake is deleted and no longer appears in lists  |  30 days after the handshake is completed  | 

### Number of policies that you can attach to an entity<a name="min-max-policies"></a>

The minimum and maximum depend on the policy type and the entity that you're attaching the policy to\. The following table shows each policy type and the number of entities that you can attach each type to\.


****  

| Policy type | Minimum attached to an entity | Maximum attached to root | Maximum attached per OU | Maximum attached per account | 
| --- | --- | --- | --- | --- | 
| Service control policy | 1 — Every entity must have at least one SCP attached at all times\. You can't remove the last SCP from an entity\. | 5 | 5 | 5 | 
| AI services opt\-out policy | 0 | 5 | 5 | 5 | 
| Backup policy | 0 | 10 | 10 | 10 | 
| Tag policy | 0 | 5 | 5 | 5 | 

**Note**  
Currently, you can have only one root in an organization\.