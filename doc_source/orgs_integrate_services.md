# Using AWS Organizations with other AWS services<a name="orgs_integrate_services"></a>

You can use *trusted access* to enable a supported AWS service that you specify, called the *trusted service*, to perform tasks in your organization and its accounts on your behalf\. This involves granting permissions to the trusted service but does *not* otherwise affect the permissions for IAM users or roles\. When you enable access, the trusted service can create an IAM role called a *service\-linked role* in every account in your organization whenever that role is needed\. That role has a permissions policy that allows the trusted service to do the tasks that are described in that service's documentation\. This enables you to specify settings and configuration details that you would like the trusted service to maintain in your organization's accounts on your behalf\. The trusted service only creates service\-linked roles when it needs to perform management actions on accounts, and not necessarily in all accounts of the organization\. <a name="important-note-about-integration"></a>

**Important**  
We ***strongly recommend*** that you enable and disable trusted access by using ***only*** the trusted service's console, or its AWS CLI or API operation equivalents\. This lets the trusted service perform any required initialization when enabling trusted access, such as creating any required resources and any required clean up of resources when disabling trusted access\.  
For information about how to enable or disable trusted service access to your organization using the trusted service, see the **Learn more** link under the **Supports Trusted Access** column at [AWS services that you can use with AWS Organizations](orgs_integrate_services_list.md)\.   
If you disable access by using the Organizations console, CLI commands, or API operations, it causes the following actions to occur:  
The service can no longer create a service\-linked role in the accounts in your organization\. This means that the service can't perform operations on your behalf on any new accounts in your organization\. The service can still perform operations in older accounts until the service completes its clean\-up from AWS Organizations\. 
The service can no longer perform tasks in the member accounts in the organization, unless those operations are explicitly permitted by the IAM policies that are attached to your roles\. This includes any data aggregation from the member accounts to the management account, or to a delegated administrator account, where relevant\.
Some services detect this and clean up any remaining data or resources related to the integration, while other services stop accessing the organization but leave any historical data and configuration in place to support a possible re\-enabling of the integration\.
Instead, using the other service's console or commands to disable the integration ensures that the other service can clean up any resources that are required only for the integration\. How the service cleans up its resources in the organization's accounts depends on that service\. For more information, see the documentation for the other AWS service\. 

## Permissions required to enable trusted access<a name="orgs_trusted_access_perms"></a>

Trusted access requires permissions for two services: AWS Organizations and the trusted service\. To enable trusted access, choose one of the following scenarios:
+ If you have credentials with permissions in both AWS Organizations and the trusted service, enable access by using the tools \(console or AWS CLI\) provided by the trusted service\. This allows the service to enable trusted access in AWS Organizations on your behalf and to create any resources required for the service to operate in your organization\.

  The minimum permissions for these credentials are the following:
  + `organizations:EnableAWSServiceAccess`\. You can also use the `organizations:ServicePrincipal` condition key with this operation to limit requests that those operations make to a list of approved service principal names\. For more information, see [Condition keys](orgs_permissions_overview.md#orgs_permissions_conditionkeys)\.
  + `organizations:ListAWSServiceAccessForOrganization` – Required if you use the AWS Organizations console\.
  + The minimum permissions that are required by the trusted service depend on the service\. For more information, see the trusted service's documentation\.
+ If one person has credentials with permissions in AWS Organizations but someone else has credentials with permissions in the trusted service, perform these steps in the following order:

  1. The person who has credentials with permissions in AWS Organizations should use the AWS Organizations console, the AWS CLI, or an AWS SDK to enable trusted access for the trusted service\. This grants permission to the other service to perform its required configuration in the organization when the following step \(step 2\) is performed\.

     The minimum AWS Organizations permissions are the following:
     + `organizations:EnableAWSServiceAccess`
     + `organizations:ListAWSServiceAccessForOrganization` – Required only if you use the AWS Organizations console

     For the steps to enable trusted access in AWS Organizations, see [How to enable or disable trusted access](#orgs_how-to-enable-disable-trusted-access)\.

  1. The person who has credentials with permissions in the trusted service enables that service to work with AWS Organizations\. This instructs the service to perform any required initialization, such as creating any resources that are required for the trusted service to operate in the organization\. For more information, see the service\-specific instructions at [AWS services that you can use with AWS Organizations](orgs_integrate_services_list.md)\.

## Permissions required to disable trusted access<a name="orgs_trusted_access_disable_perms"></a>

When you no longer want to allow the trusted service to operate on your organization or its accounts, choose one of the following scenarios\.

**Important**  
Disabling trusted service access does ***not*** prevent users and roles with appropriate permissions from using that service\. To completely block users and roles from accessing an AWS service, you can remove the IAM permissions that grant that access, or you can use [service control policies \(SCPs\)](orgs_manage_policies_scps.md) in AWS Organizations\.  
You can apply SCPs to only member accounts\. SCPs don't apply to the management account\. We recommend that you [don’t run services in the management account\.](orgs_best-practices_mgmt-acct.md#best-practices_mgmt-use) Instead, run them in member accounts where you can control the security by using SCPs\.
+ If you have credentials with permissions in both AWS Organizations and the trusted service, disable access by using the tools \(console or AWS CLI\) that are available for the trusted service\. The service then cleans up by removing resources that are no longer required and by disabling trusted access for the service in AWS Organizations on your behalf\. 

  The minimum permissions for these credentials are the following:
  + `organizations:DisableAWSServiceAccess`\. You can also use the `organizations:ServicePrincipal` condition key with this operation to limit requests that those operations make to a list of approved service principal names\. For more information, see [Condition keys](orgs_permissions_overview.md#orgs_permissions_conditionkeys)\.
  + `organizations:ListAWSServiceAccessForOrganization` – Required if you use the AWS Organizations console\.
  + The minimum permissions required by the trusted service depend on the service\. For more information, see the trusted service's documentation\.
+ If the credentials with permissions in AWS Organizations aren't the credentials with permissions in the trusted service, perform these steps in the following order:

  1. The person with permissions in the trusted service first disables access using that service\. This instructs the trusted service to clean up by removing the resources required for trusted access\. For more information, see the service\-specific instructions at [AWS services that you can use with AWS Organizations](orgs_integrate_services_list.md)\.

  1. The person with permissions in AWS Organizations can then use the AWS Organizations console, AWS CLI, or an AWS SDK to disable access for the trusted service\. This removes the permissions for the trusted service from the organization and its accounts\. 

     The minimum AWS Organizations permissions are the following:
     + `organizations:DisableAWSServiceAccess`
     + `organizations:ListAWSServiceAccessForOrganization` – Required only if you use the AWS Organizations console

     For the steps to disable trusted access in AWS Organizations, see [How to enable or disable trusted access](#orgs_how-to-enable-disable-trusted-access)\.

## How to enable or disable trusted access<a name="orgs_how-to-enable-disable-trusted-access"></a>

If you have permissions only for AWS Organizations and you want to enable or disable trusted access to your organization on behalf of the administrator of the other AWS service, use the following procedure\.

**Important**  
We ***strongly recommend*** that you enable and disable trusted access by using ***only*** the trusted service's console, or its AWS CLI or API operation equivalents\. This lets the trusted service perform any required initialization when enabling trusted access, such as creating any required resources and any required clean up of resources when disabling trusted access\.  
For information about how to enable or disable trusted service access to your organization using the trusted service, see the **Learn more** link under the **Supports Trusted Access** column at [AWS services that you can use with AWS Organizations](orgs_integrate_services_list.md)\.   
If you disable access by using the Organizations console, CLI commands, or API operations, it causes the following actions to occur:  
The service can no longer create a service\-linked role in the accounts in your organization\. This means that the service can't perform operations on your behalf on any new accounts in your organization\. The service can still perform operations in older accounts until the service completes its clean\-up from AWS Organizations\. 
The service can no longer perform tasks in the member accounts in the organization, unless those operations are explicitly permitted by the IAM policies that are attached to your roles\. This includes any data aggregation from the member accounts to the management account, or to a delegated administrator account, where relevant\.
Some services detect this and clean up any remaining data or resources related to the integration, while other services stop accessing the organization but leave any historical data and configuration in place to support a possible re\-enabling of the integration\.
Instead, using the other service's console or commands to disable the integration ensures that the other service can clean up any resources that are required only for the integration\. How the service cleans up its resources in the organization's accounts depends on that service\. For more information, see the documentation for the other AWS service\. 

------
#### [ AWS Management Console ]

**To enable trusted service access**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for the service that you want to enable, and choose its name\.

1. Choose **Enable trusted access**\.

1. In the confirmation dialog box, check the box to **Show the option to enable trusted access**, enter **enable** in the box, and then choose **Enable trusted access**\.

1. If you are *enabling* access, tell the administrator of the other AWS service that they can now enable the other service to work with AWS Organizations\.

**To disable trusted service access**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for the service that you want to disable, and choose its name\.

1. Wait until the administrator of the other service tells you that the service is disabled and that its resources have been cleaned up\.

1. In the confirmation dialog box, enter **disable** in the box, and then choose **Disable trusted access**\.

------
#### [ AWS CLI, AWS API ]

**To enable or disable trusted service access**  
You can use the following AWS CLI commands or API operations to enable or disable trusted service access:
+ AWS CLI: AWS organizations [enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)
+ AWS CLI: AWS organizations [disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------

## AWS Organizations and service\-linked roles<a name="orgs_integrate_services-using_slrs"></a>

AWS Organizations uses [IAM service\-linked roles](http://aws.amazon.com/blogs/security/introducing-an-easier-way-to-delegate-permissions-to-aws-services-service-linked-roles/) to enable trusted services to perform tasks on your behalf in your organization's member accounts\. When you configure a trusted service and authorize it to integrate with your organization, that service can request that AWS Organizations create a service\-linked role in its member account\. The trusted service does this asynchronously as needed and not necessarily in all accounts in the organization at the same time\. The service\-linked role has predefined IAM permissions that allow the trusted service to perform only specific tasks within that account\. In general, AWS manages all service\-linked roles, which means that you typically can't alter the roles or the attached policies\. 

To make all of this possible, when you create an account in an organization or you accept an invitation to join your existing account to an organization, AWS Organizations provisions the member account with a service\-linked role named `AWSServiceRoleForOrganizations`\. Only the AWS Organizations service itself can assume this role\. The role has permissions that allow AWS Organizations to create service\-linked roles for other AWS services\. This service\-linked role is present in all organizations\.

Although we don't recommend it, if your organization has only [consolidated billing features](orgs_getting-started_concepts.md#feature-set-cb-only) enabled, the service\-linked role named `AWSServiceRoleForOrganizations` is never used, and you can delete it\. If you later want to enable [all features](orgs_getting-started_concepts.md#feature-set-all) in your organization, the role is required, and you must restore it\. The following checks occur when you begin the process to enable all features:
+ **For each member account that was *invited to join* the organization** – The account administrator receives a request to agree to enable all features\. To successfully agree to the request, the administrator must have both `organizations:AcceptHandshake` *and* `iam:CreateServiceLinkedRole` permissions if the service\-linked role \(`AWSServiceRoleForOrganizations`\) doesn't already exist\. If the `AWSServiceRoleForOrganizations` role already exists, the administrator needs only the `organizations:AcceptHandshake` permission to agree to the request\. When the administrator agrees to the request, AWS Organizations creates the service\-linked role if it doesn't already exist\. 
+ **For each member account that was *created* in the organization** – The account administrator receives a request to recreate the service\-linked role\. \(The administrator of the member account doesn't receive a request to enable all features because the administrator of the management account \(formerly known as the "master account"\) is considered the owner of the created member accounts\.\) AWS Organizations creates the service\-linked role when the member account administrator agrees to the request\. The administrator must have both `organizations:AcceptHandshake` *and* `iam:CreateServiceLinkedRole` permissions to successfully accept the handshake\.

After you enable all features in your organization, you no longer can delete the `AWSServiceRoleForOrganizations` service\-linked role from any account\.

**Important**  
AWS Organizations SCPs never affect service\-linked roles\. These roles are exempt from any SCP restrictions\.