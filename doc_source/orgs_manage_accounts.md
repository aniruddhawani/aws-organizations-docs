# Managing the AWS accounts in your organization<a name="orgs_manage_accounts"></a>

An organization is a collection of AWS accounts that you manage together\. You can perform the following tasks to manage the accounts that are part of your organization:
+ [View details of the accounts in your organization](orgs_manage_org_details.md#orgs_view_account)\. You can see the account's unique ID number, its Amazon Resource Name \(ARN\), and the policies that are attached to it\.
+ [Export a list of all AWS accounts in your organization](orgs_manage_accounts_export.md)\. You can download a \.csv file that contains account details for every account within your organization\.
+ [Invite existing AWS accounts to join your organization](orgs_manage_accounts_invites.md)\. Create invitations, manage invitations that you have created, and accept or decline invitations\.
+ [Create an AWS account as part of your organization](orgs_manage_accounts_create.md)\. Create and access an AWS account that is automatically part of your organization\.
+ [Update alternate contacts in your organization](orgs_manage_accounts_update_contacts.md)\. Update alternate contacts for your AWS accounts in your organization\.
+ [Remove an AWS account from your organization](orgs_manage_accounts_remove.md)\. As an administrator in the management account, remove member accounts that you no longer want to manage from your organization\. As an administrator of a member account, remove your account from its organization\. If the management account has attached a policy to your member account, you could be blocked from removing your account\. 
+ [Delete \(or close\) an AWS account](orgs_manage_accounts_close.md)\. When you no longer need an AWS account, you can close the account to prevent any usage or accrual of charges\.

## Impact of being in an organization<a name="orgs-account-join-faq"></a>
+ [What is the impact on an AWS account that ***joins*** an organization?](#impact_of_join)
+ [What is the impact on an AWS account that you ***create*** in an organization?](#impact_of_create)

### Impact on an AWS account that joins an organization?<a name="impact_of_join"></a>

When you invite an AWS account to join an organization, and the owner of the account accepts the invitation, AWS Organizations automatically makes the following changes to the new member account:
+ AWS Organizations creates a service\-linked role called `AWSServiceRoleForOrganizations`\. The account must have this role if your organization supports all features\. You can delete the role if the organization supports only the consolidated billing feature set\. If you delete the role and later you enable all features in your organization, AWS Organizations recreates the role for the account\. 
+ You might have a variety of policies attached to the organization root or the OU that contains the account\. If so, those policies immediately apply to all users and roles in the invited account\.
+ You can [enable service trust for another AWS service](orgs_integrate_services_list.md) for your organization\. When you do, that trusted service can create service\-linked roles or perform actions in any member account in the organization, including an invited account\.

**Note**  
For invited member accounts, AWS Organizations doesn't automatically create the IAM role [OrganizationAccountAccessRole](orgs_manage_accounts_access.md#orgs_manage_accounts_access-cross-account-role)\. This role grants users in the management account administrative access to the member account\. If you want to enable that level of administrative control to an invited account, you can manually add the role\. For more information, see [Creating the OrganizationAccountAccessRole in an invited member account](orgs_manage_accounts_access.md#orgs_manage_accounts_create-cross-account-role)\.

You can invite an account to join an organization that has only the consolidated billing features enabled\. If you later want to enable all features for the organization, invited accounts must approve the change\.

### Impact on an AWS account that you create in an organization?<a name="impact_of_create"></a>

When you create an AWS account in your organization, AWS Organizations automatically makes the following changes to the new member account:
+ AWS Organizations creates a service\-linked role called `AWSServiceRoleForOrganizations`\. The account must have this role if your organization supports all features\. You can delete the role if the organization supports only the consolidated billing feature set\. If you delete the role and later you enable all features in your organization, AWS Organizations recreates the role for the account\.
+ AWS Organizations creates the IAM role [OrganizationAccountAccessRole](orgs_manage_accounts_access.md#orgs_manage_accounts_access-cross-account-role)\. This role grants the management account access to the new member account\. Although this role *can* be deleted, we recommend that you don't delete it so that it is available as a recovery option\.
+ If you have any [policies attached to the root of the OU tree](orgs_manage_policies.md), those policies immediately apply to all users and roles in the created account\. New accounts are added to the root OU by default\.
+ If you have [enabled service trust for another AWS service](orgs_integrate_services_list.md) for your organization, that trusted service can create service\-linked roles or perform actions in any member account in the organization, including your created account\.