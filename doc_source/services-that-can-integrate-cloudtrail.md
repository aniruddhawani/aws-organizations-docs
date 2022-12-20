# AWS CloudTrail and AWS Organizations<a name="services-that-can-integrate-cloudtrail"></a>

AWS CloudTrail is an AWS service that helps you enable governance, compliance, and operational and risk auditing of your AWS account\. Using AWS CloudTrail, a user in a management account can create an organization trail that logs all events for all AWS accounts in that organization\. Organization trails are automatically applied to all member accounts in the organization\. Member accounts can see the organization trail, but can't modify or delete it\. By default, member accounts don't have access to the log files for the organization trail in the Amazon S3 bucket\. This helps you uniformly apply and enforce your event logging strategy across the accounts in your organization\.

For more information, see [ Creating a Trail for an Organization](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/creating-trail-organization.html) in the *AWS CloudTrail User Guide*\. 

Use the following information to help you integrate AWS CloudTrail with AWS Organizations\.



## Service\-linked roles created when you enable integration<a name="integrate-enable-slr-cloudtrail"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your organization's management account when you enable trusted access\. This role allows CloudTrail to perform supported operations within your organization's accounts in your organization\.

You can delete or modify this role only if you disable trusted access between CloudTrail and Organizations, or if you remove the member account from the organization\.
+ `AWSServiceRoleForCloudTrail`

## Service principals used by the service\-linked roles<a name="integrate-enable-svcprin-cloudtrail"></a>

The service\-linked role in the previous section can be assumed only by the service principals authorized by the trust relationships defined for the role\. The service\-linked roles used by CloudTrail grant access to the following service principals:
+ `cloudtrail.amazonaws.com`

## Enabling trusted access with CloudTrail<a name="integrate-enable-ta-cloudtrail"></a>

For information about the permissions needed to enable trusted access, see [Permissions required to enable trusted access](orgs_integrate_services.md#orgs_trusted_access_perms)\.

You can enable trusted access using either the AWS CloudTrail console or the AWS Organizations console\.

**Important**  
We strongly recommend that whenever possible, you use the AWS CloudTrail console or tools to enable integration with Organizations\. This lets AWS CloudTrail perform any configuration that it requires, such as creating resources needed by the service\. Proceed with these steps only if you can’t enable integration using the tools provided by AWS CloudTrail\. For more information, see [this note](orgs_integrate_services.md#important-note-about-integration)\.   
If you enable trusted access by using the AWS CloudTrail console or tools then you don’t need to complete these steps\.

You must sign in with your AWS Organizations management account to create an organization trail\. If you create the trail from the AWS CloudTrail console, trusted access is configured automatically for you\. If you choose to create an organization trail using the AWS CLI or the AWS API, you must manually configure trusted access\. For more information, see [ Enabling CloudTrail as a trusted service in AWS Organizations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-an-organizational-trail-by-using-the-aws-cli.html#cloudtrail-create-organization-trail-by-using-the-cli-enable-trusted-service) in the *AWS CloudTrail User Guide\.*

You can enable trusted access by running a Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS CLI, AWS API ]

**To enable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to enable trusted service access:
+ AWS CLI: [enable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/enable-aws-service-access.html)

  You can run the following command to enable AWS CloudTrail as a trusted service with Organizations\.

  ```
  $ aws organizations enable-aws-service-access \
      --service-principal cloudtrail.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [EnableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_EnableAWSServiceAccess.html)

------

## Disabling trusted access with CloudTrail<a name="integrate-disable-ta-cloudtrail"></a>

For information about the permissions needed to disable trusted access, see [Permissions required to disable trusted access](orgs_integrate_services.md#orgs_trusted_access_disable_perms)\.

AWS CloudTrail requires trusted access with AWS Organizations to work with organization trails\. If you disable trusted access using AWS Organizations while you're using AWS CloudTrail for organization trails, the trails stop functioning for member accounts because CloudTrail can't access the organization\. The organization trails remain, as does the `AWSServiceRoleForCloudTrail` role created for integration between CloudTrail and AWS Organizations\. If you re\-enable trusted access, CloudTrail continues to operate as before, without the need for you to reconfigure the trails\.

Only an administrator in the AWS Organizations management account can disable trusted access with AWS CloudTrail\.

You can disable trusted access using only the Organizations tools\.

You can disable trusted access by using either the AWS Organizations console, by running an Organizations AWS CLI command, or by calling an Organizations API operation in one of the AWS SDKs\.

------
#### [ AWS Management Console ]

**To disable trusted service access using the Organizations console**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[Services](https://console.aws.amazon.com/organizations/v2/home/services)** page, find the row for **AWS CloudTrail** and then choose the service’s name\.

1. Choose **Disable trusted access**\.

1. In the confirmation dialog box, enter **disable** in the box, and then choose **Disable trusted access**\.

1. If you are the administrator of only AWS Organizations, tell the administrator of AWS CloudTrail that they can now disable that service using its console or tools from working with AWS Organizations\.

------
#### [ AWS CLI, AWS API ]

**To disable trusted service access using the Organizations CLI/SDK**  
You can use the following AWS CLI commands or API operations to disable trusted service access:
+ AWS CLI: [disable\-aws\-service\-access](https://docs.aws.amazon.com/cli/latest/reference/organizations/disable-aws-service-access.html)

  You can run the following command to disable AWS CloudTrail as a trusted service with Organizations\.

  ```
  $ aws organizations disable-aws-service-access \
      --service-principal cloudtrail.amazonaws.com
  ```

  This command produces no output when successful\.
+ AWS API: [DisableAWSServiceAccess](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DisableAWSServiceAccess.html)

------

## Enabling a delegated administrator account for CloudTrail<a name="integrate-enable-da-cloudtrail"></a>

When you use CloudTrail with Organizations, you can register any account within the organization to act as a CloudTrail delegated administrator to manage the organization's trails and event data stores on behalf of the organization\. A delegated administrator is a member account in an organization that can perform the same administrative tasks in CloudTrail as the management account\. 

**Minimum permissions**  
Only an administrator in the Organizations management account can register a delegated administrator for CloudTrail\.

You can register a delegated administrator account using the CloudTrail console, or by using the Organizations `RegisterDelegatedAdministrator` CLI or SDK operation\. To register a delegated administrator using the CloudTrail console, see [ Add a CloudTrail delegated administrator](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-add-delegated-administrator.html)\. 

## Disabling a delegated administrator for CloudTrail<a name="integrate-disable-da-cloudtrail"></a>

 Only an administrator in the Organizations management account can remove a delegated administrator for CloudTrail\. You can remove the delegated administrator using either the CloudTrail console, or by using the Organizations `DeregisterDelegatedAdministrator` CLI or SDK operation\. For information on how to remove a delegated administrator using the CloudTrail console, see [Remove a CloudTrail delegated administrator](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-remove-delegated-administrator.html) \. 