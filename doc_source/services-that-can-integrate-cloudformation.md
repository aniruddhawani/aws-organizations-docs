# AWS CloudFormation StackSets and AWS Organizations<a name="services-that-can-integrate-cloudformation"></a>

AWS CloudFormation StackSets enables you to create, update, or delete stacks across multiple AWS accounts and AWS Regions with a single operation\. StackSets integration with AWS Organizations enables you to create stack sets with service\-managed permissions, using a service\-linked role that has the relevant permission in each member account\. This lets you deploy stack instances to member accounts in your organization\. You don't have to create the necessary AWS Identity and Access Management roles; StackSets creates the IAM role in each member account on your behalf\. You can also choose to enable automatic deployments to accounts that are added to your organization in the future\.

With trusted access between StackSets and Organizations enabled, the management account has permissions to create and manage stack sets for your organization\. The management account can register up to five member accounts as delegated administrators\. With trusted access enabled, delegated administrators also have permissions to create and manage stack sets for your organization\. Stack sets with service\-managed permissions are created in the management account, including stack sets that are created by delegated administrators\.

**Important**  
Delegated administrators have full permissions to deploy to accounts in your organization\. The management account cannot limit delegated administrator permissions to deploy to specific OUs or to perform specific stack set operations\.

 For more information about integrating StackSets with Organizations, see [Working with AWS CloudFormation StackSets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/what-is-cfnstacksets.html) in the *AWS CloudFormation User Guide*\.

Use the following information to help you integrate AWS CloudFormation StackSets with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-cloudformation"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's management account when you enable trusted access\. This role allows AWS CloudFormation Stacksets to perform supported operations within your organization's accounts in your organization\.

You can delete or modify this role only if you disable trusted access between AWS CloudFormation Stacksets and Organizations, or if you remove the member account from the organization\.
+ Management account: `AWSServiceRoleForCloudFormationStackSetsOrgAdmin`

To create the service\-linked role `AWSServiceRoleForCloudFormationStackSetsOrgMember` for the member accounts in your organization, you need to create a stack set in the management account first\. This creates a stack set instance, which then creates the role in the member account\.
+ Member accounts: `AWSServiceRoleForCloudFormationStackSetsOrgMember`

For more details about creating stack sets, see [Working with AWS CloudFormation StackSets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/what-is-cfnstacksets.html) in the *AWS CloudFormation User Guide*\.

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-cloudformation"></a>

The service\-linked role in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by AWS CloudFormation Stacksets grant access to the following service principals:
+ Management account: `stacksets.cloudformation.amazonaws.com`

  You can modify or delete this role only if you disabled trusted access between StackSets and Organizations\.
+ Member accounts: `member.org.stacksets.cloudformation.amazonaws.com`

  You can modify or delete this role from an account only if you first disable trusted access between StackSets and Organizations, or if you first remove the account from the target organization or organizational unit \(OU\)\.

## Enabling trusted access with AWS CloudFormation Stacksets<a name="integrate-enable-ta-cloudformation"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

Only an administrator in the Organizations management account has permissions to enable trusted access with another AWS service\. You can enable trusted access using either the AWS CloudFormation console or the Organizations console\.

You can enable trusted access using only AWS CloudFormation StackSets\.

To enable trusted access using the AWS CloudFormation Stacksets console, see [Enable Trusted Access with AWS Organizations](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-orgs-enable-trusted-access.html) in the AWS CloudFormation User Guide\.

## Disabling trusted access with AWS CloudFormation Stacksets<a name="integrate-disable-ta-cloudformation"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

Only an administrator in an Organizations management account has permissions to disable trusted access with another AWS service\. You can disable trusted access only by using the Organizations console\. If you disable trusted access with Organizations while you are using StackSets, all previously created stack instances are retained\. However, stack sets deployed using the service\-linked role's permissions can no longer perform deployments to accounts managed by Organizations\. 

You can disable trusted access using either the AWS CloudFormation console or the Organizations console\.

**Important**  
If you disable trusted access programmatically \(e\.g with AWS CLI or with an API\), be aware that this will remove the permission\. It is better to disable trusted access with the AWS CloudFormation console\. 

You can disable trusted access by using either the AWS Organizations console, by running an Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS CloudFormation StackSets** and then choose the service’s name\.

1. Choose **Disable trusted access**\.

1. In the confirmation dialog box, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS CloudFormation StackSets that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS CloudFormation StackSets as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal stacksets.cloudformation.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------

## Enabling a delegated administrator account for AWS CloudFormation Stacksets<a name="integrate-enable-da-cloudformation"></a>

When you designate a member account as a delegated administrator for the organization, users and roles from that account can perform administrative actions for AWS CloudFormation Stacksets that otherwise can be performed only by users or roles in the organization's management account\. This helps you to separate management of the organization from management of AWS CloudFormation Stacksets\.

For instructions on how to designate a member account as a delegated administrator of AWS CloudFormation Stacksets in the organization, see [Register a delegated administrator](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-orgs-delegated-admin.html) in the *AWS CloudFormation User Guide*\.