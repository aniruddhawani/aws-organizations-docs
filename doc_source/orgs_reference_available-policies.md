# AWS managed policies available for use with AWS Organizations<a name="orgs_reference_available-policies"></a>

This section identifies the AWS\-managed policies provided for your use to manage your organization\. You can't modify or delete an AWS managed policy, but you can attach or detach them to entities in your organization as needed\.

## AWS Organizations managed policies for use with AWS Identity and Access Management \(IAM\)<a name="ref-iam-managed-policies"></a>

An IAM managed policy is provided and maintained by AWS\. A managed policy provides permissions for common tasks that you can assign to your users by attaching the managed policy to the appropriate IAM user or role object\. You don't have to write the policy yourself, and when AWS updates the policy as appropriate to support new services, you automatically and immediately get the benefit of the update\. You can see the list of AWS managed policies in [Policies](https://console.aws.amazon.com/iam/home?#/policies) page on the IAM console\. Use the **Filter policies** drop\-down to select **AWS managed**\. 

You can use the following managed policies to grant permissions to users in your organization\.


****  

| Policy name | Description | ARN | 
| --- | --- | --- | 
| [AWSOrganizationsFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AWSOrganizationsFullAccess$jsonEditor) | Provides all of the permissions required to create and fully administer an organization\. The content of this policy statement is shown in the following snippet:<pre>{<br />    "Version": "2012-10-17",<br />    "Statement": [<br />        {<br />            "Effect": "Allow",<br />            "Action": "organizations:*",<br />            "Resource": "*"<br />        },<br />        {<br />            "Effect": "Allow",<br />            "Action": [<br />                "account:PutAlternateContact",<br />                "account:DeleteAlternateContact",<br />                "account:GetAlternateContact"<br />            ],<br />            "Resource": "*"<br />        },<br />        {<br />            "Effect": "Allow",<br />            "Action": [<br />                "account:GetContactInformation",<br />                "account:PutContactInformation"<br />            ],<br />            "Resource": "*"<br />        },<br />        {<br />            "Effect": "Allow",<br />            "Action": "iam:CreateServiceLinkedRole",<br />            "Resource": "*",<br />            "Condition": {<br />                "StringEquals": {<br />                    "iam:AWSServiceName": "organizations.amazonaws.com"<br />                }<br />            }<br />        }<br />    ]<br />}</pre> | arn:aws:iam::aws:policy/AWSOrganizationsFullAccess | 
| [AWSOrganizationsReadOnlyAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AWSOrganizationsReadOnlyAccess$jsonEditor) | Provides read only access to information about the organization\. It doesn't permit the user to make any changes\. The content of this policy statement is shown in the following snippet: <pre>{<br />   "Version":"2012-10-17",<br />   "Statement":[<br />      {<br />         "Effect":"Allow",<br />         "Action":[<br />            "organizations:Describe*",<br />            "organizations:List*"<br />         ],<br />         "Resource":"*"<br />      },<br />      {<br />         "Effect":"Allow",<br />         "Action":[<br />            "account:GetAlternateContact"<br />         ],<br />         "Resource":"*"<br />      },<br />      {<br />         "Effect":"Allow",<br />         "Action":[<br />            "account:GetContactInformation"<br />         ],<br />         "Resource":"*"<br />      }<br />   ]<br />}</pre> | arn:aws:iam::aws:policy/AWSOrganizationsReadOnlyAccess | 

### Updates to Organizations AWS managed policies<a name="ref-iam-managed-policies-updates"></a>

The following table details updates to AWS managed policies since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the [AWS Organizations Document History page](document-history.md)\.


****  

| Change | Description | Date | 
| --- | --- | --- | 
|  [AWSOrganizationsFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AWSOrganizationsFullAccess$jsonEditor) – updated to allow account API permissions required to add or edit account contacts via the Organizations console\.  |  Organizations added the `account:GetContactInformation` and `account:PutContactInformation` action to the policy to enable write access to modify contacts for an account\.  |  October 21, 2022  | 
|  [AWSOrganizationsReadOnlyAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AWSOrganizationsReadOnlyAccess$jsonEditor) – updated to allow account API permissions required to view account contacts via the Organizations console\.  |  Organizations added the `account:GetContactInformation` action to the policy to enable access to view contacts for an account\.  |  October 21, 2022  | 
|  [AWSOrganizationsFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AWSOrganizationsFullAccess$jsonEditor) – updated to allow creating an organization\.  |  Organizations added the `CreateServiceLinkedRole` permission to the policy to enable creating the service linked role required to create an organization\. The permission is restricted to creating a role that can be used only by the `organizations.amazonaws.com` service\.  |  August 24, 2022  | 
|  [AWSOrganizationsFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AWSOrganizationsFullAccess$jsonEditor) – updated to allow account API permissions required to add, edit, or delete account alternate contacts via the Organizations console\.  |  Organizations added the `account:GetAlternateContact`, `account:DeleteAlternateContact`, `account:PutAlternateContact` actions to the policy to enable write access to modify alternate contacts for an account\.  |  February 7, 2022  | 
|  [AWSOrganizationsReadOnlyAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AWSOrganizationsReadOnlyAccess$jsonEditor) – updated to allow account API permissions required to view account alternate contacts via the Organizations console\.  |  Organizations added the `account:GetAlternateContact` action to the policy to enable access to view alternate contacts for an account\.  |  February 7, 2022  | 

## AWS Organizations managed service control policies<a name="ref-managed-scp-policies"></a>

[Service control policies \(SCPs\)](orgs_manage_policies_scps.md) are similar to IAM permission policies, but are a feature of AWS Organizations rather than IAM\. You use SCPs to specify maximum permissions for affected entities\. You can attach SCPs to roots, organizational units \(OUs\), or accounts in your organization\. You can create your own, or you can use the policies that IAM defines\. You can see the list of policies in your organization on the [Policies](https://console.aws.amazon.com/organizations/?#/policies) page on the Organizations console\.

**Important**  
Every root, OU, and account must have at least one SCP attached at all times\.


****  

| Policy name | Description | ARN | 
| --- | --- | --- | 
| [FullAWSAccess](https://console.aws.amazon.com/organizations/?#/policies/p-FullAWSAccess) | Provides AWS Organizations management account access to member accounts\. | arn:aws:organizations::aws:policy/service\_control\_policy/p\-FullAWSAccess | 