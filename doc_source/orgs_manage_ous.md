# Managing organizational units \(OUs\)<a name="orgs_manage_ous"></a>

You can use organizational units \(OUs\) to group accounts together to administer as a single unit\. This greatly simplifies the management of your accounts\. For example, you can attach a policy\-based control to an OU, and all accounts within the OU automatically inherit the policy\. You can create multiple OUs within a single organization, and you can create OUs within other OUs\. Each OU can contain multiple accounts, and you can move accounts from one OU to another\. However, OU names must be unique within a parent OU or root\.

**Note**  
There is one root in the organization, which AWS Organizations creates for you when you first set up your organization\.

To structure the accounts in your organization, you can perform the following tasks:
+ [Viewing details of an OU](orgs_manage_org_details.md#orgs_view_ou) 
+ [Creating an OU](#create_ou)
+ [Renaming an OU](#rename_ou)
+ [Edit tags attached to an OU](#tag_ou)
+ [Moving an account to an OU or between the root and OUs](#move_account_to_ou)

## Navigating the root and OU hierarchy<a name="navigate_tree"></a>

To navigate to different OUs or to the root when moving accounts or attaching policies, you can use the default "tree" view\.

------
#### [ AWS Management Console ]

**To navigate the organization as a 'tree'**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, at the top of the **Organization** section, select the **Hierarchy** toggle \(instead of **List**\)\.

1. The tree initially appears showing the root, displaying only the first level of child OUs and accounts\. To expand the tree to show deeper levels, choose the expand icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-expand.png)\)next to any parent entity\. To reduce clutter and collapse a branch of the tree, choose the collapse icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-collapse.png)\) next to an expanded parent entity\.

1. Choose the name of an OU or root to view its details and perform certain operations\. Alternatively, you can choose the radio button next to the name, and perform certain operations on that entity in the **Actions** menu\.

------

You can also view the list of only the accounts in your organization in tabular form, without having to first navigate to an OU to find them\. In this view you can't see any of the OUs or manipulate the policies attached to them\.

------
#### [ AWS Management Console ]

**To view the organization as a flat list of accounts with no hierarchy**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, at the top of the **Organization** section, choose the **View AWS accounts only** switch icon to turn it on\. ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/console-switch-on.png)\.

1. The list of accounts is displayed without any hierarchy\.

------

## Creating an OU<a name="create_ou"></a>

When you sign in to your organization's management account, you can create an OU in your organization's root\. OUs can be nested up to five levels deep\. To create an OU, complete the following steps\.

**Important**  
If this organization is managed with AWS Control Tower, then create your OUs with the AWS Control Tower console or APIs\. If you create the OU in Organizations, then that OU isn't registered with AWS Control Tower\. For more information, see [Referring to Resources Outside of AWS Control Tower](https://docs.aws.amazon.com/controltower/latest/userguide/external-resources.html#ungoverned-resources) in the *AWS Control Tower User Guide*\.

**Minimum permissions**  
To create an OU within a root in your organization, you must have the following permissions:  
`organizations:DescribeOrganization` – required only when using the Organizations console
`organizations:CreateOrganizationalUnit`

------
#### [ AWS Management Console ]

**To create an OU**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. Navigate to the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page\.

   The console displays the Root OU and its contents\. The first time you visit the Root, the console displays all of your AWS accounts in that top\-level view\. If you previously created OUs and moved accounts into them, the console shows only the top\-level OUs and any accounts that you have not yet moved into an OU\.

1. \(Optional\) If you want to create an OU inside an existing OU, [navigate to the child OU](#navigate_tree) by choosing the name \(not the check box\) of the child OU, or by choosing the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/expand-icon.png) next to OUs in the tree view until you see the one you want, and then choosing its name\.

1. When you've selected the correct parent OU in the hierarchy, on the **Actions** menu, under **Organizational Unit**, choose **Create new**

1. In the **Create organizational unit** dialog box, enter the name of the OU that you want to create\.

1. \(Optional\) Add one or more tags by choosing **Add tag** and then entering a key and an optional value\. Leaving the value blank sets it to an empty string; it isn't `null`\. You can attach up to 50 tags to an OU\.

1. Finally, choose **Create organizational unit**\.

Your new OU appears inside the parent\. You now can [move accounts to this OU](#move_account_to_ou) or attach policies to it\.

------
#### [ AWS CLI & AWS SDKs ]

**To create an OU**  
You can use one of the following commands to create an OU:
+ AWS CLI: [create\-organizational\-unit](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-organizational-unit.html)

  To create an OU, you must first find the identity of the root or OU that you want to be the parent of the new OU\.

  To find the identity of the root, use the [list\-roots](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-roots.html) command\. To find the identity of an OU, use the [list\-children](https://docs.aws.amazon.com/cli/latest/reference/organizations/list-children.html) to navigate to the OU you want\.

  The following example shows how to find the identity of the root, and then find the identity of an OU under the root\. The last command shows how to create a new OU in that found OU\.

  ```
  $ aws organizations list-roots
  {
      "Roots": [
          {
              "Id": "r-a1b2",
              "Arn": "arn:aws:organizations::123456789012:root/o-aa111bb222/r-a1b2",
              "Name": "Root",
              "PolicyTypes": []
          }
      ]
  }
  $ aws organizations list-children \
      --parent-id r-a1b2 \
      --child-type ORGANIZATIONAL_UNIT
  {
      "Children": [
          {
              "Id": "ou-a1b2-f6g7h111",
              "Type": "ORGANIZATIONAL_UNIT"
          }
      ]
  }
  $ aws organizations create-organizational-unit \
      --parent-id ou-a1b2-f6g7h111 \
      --name New-Child-OU
  {
      "OrganizationalUnit": {
          "Id": "ou-a1b2-f6g7h222",
          "Arn": "arn:aws:organizations::123456789012:ou/o-aa111bb222/ou-a1b2-f6g7h222",
          "Name": "New-Child-OU"
      }
  }
  ```
+ AWS SDKs: [CreateOrganizationalUnit](https://docs.aws.amazon.com/organizations/latest/APIReference/API_CreateOrganizationalUnit.html)

------

## Renaming an OU<a name="rename_ou"></a>

When you sign in to your organization's management account, you can rename an OU\. To do this, complete the following steps\.

**Minimum permissions**  
To rename an OU within a root in your AWS organization, you must have the following permissions:  
`organizations:DescribeOrganization` – required only when using the Organizations console
`organizations:UpdateOrganizationalUnit`

------
#### [ AWS Management Console ]

**To rename an OU**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, [navigate to the OU](#navigate_tree) that you want to rename, and then do one of the following steps:
   + Choose the radio button ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/radio-button-selected.png) next to the OU that you want to rename\. Then, on the **Actions** menu, under **Organizational unit**, choose **Rename**\.
   + Choose the OU's name, to access the OU's detail page\. Then, at the top of the page choose **Rename**\.

1. In the **Rename organizational unit** dialog box, enter a new name, and then choose **Save changes**\.

------
#### [ AWS CLI & AWS SDKs ]

**To rename an OU**  
You can use one of the following commands to rename an OU:
+ AWS CLI: [update\-organizational\-unit](https://docs.aws.amazon.com/cli/latest/reference/organizations/update-organizational-unit.html)

  The following example shows how to rename an OU\.

  ```
  $ aws organizations update-organizational-unit \
      --organizational-unit-id ou-a1b2-f6g7h222 \
      --name "Renamed-OU"
  {
      "OrganizationalUnit": {
          "Id": "ou-a1b2-f6g7h222",
          "Arn": "arn:aws:organizations::123456789012:ou/o-aa111bb222/ou-a1b2-f6g7h222",
          "Name": "Renamed-OU"
      }
  }
  ```
+ AWS SDKs: [UpdateOrganizationalUnit](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UpdateOrganizationalUnit.html)

------

## Editing tags attached to an OU<a name="tag_ou"></a>

When you sign in to your organization's management account, you can add or remove the tags attached to an OU\. To do this, complete the following steps\.

**Minimum permissions**  
To edit the tags attached to an OU within a root in your AWS organization, you must have the following permissions:  
`organizations:DescribeOrganization` – required only when using the Organizations console
`organizations:DescribeOrganizationalUnit`– required only when using the Organizations console
`organizations:TagResource`
`organizations:UntagResource`

------
#### [ AWS Management Console ]

**To edit the tags attached to an OU**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, [navigate to and choose the name of the OU](#navigate_tree) whose tags you want to edit\.

1. On the OU's details page, choose the **Tags** tab, and then choose **Manage tags**\.

1. You can perform any of these actions on this tab:
   + Edit the value for any tag by entering a new value over the old one\. You can't modify the tag key\. To change a key, you must delete the tag with the old key and add a tag with the new key\. 
   + Remove an existing tag by choosing **Remove** next to the tag you want to remive\.
   + Add a new tag key and value pair\. Choose **Add tag**, then enter the new key name and optional value in the provided boxes\. If you leave the **Value** box empty, the value is an empty string; it isn't `null`\.

1. Choose **Save changes** after you've made all the additions, removals, and edits you want to make\.

------
#### [ AWS CLI & AWS SDKs ]

**To edit the tags attached to an OU**  
You can use one of the following commands to change the tags attached to an OU:
+ AWS CLI: [tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/tag-resource.html) and [untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/organizations/untag-resource.html)

  The following example attaches the tag `"Department"="12345"` to an OU\. Note that `Key` and `Value` are case sensitive\.

  ```
  $ aws organizations tag-resource \
      --resource-id ou-a1b2-f6g7h222 \
      --tags Key=Department,Value=12345
  ```

  This command produces no output when successful\.

  The following example removes the `Department` tag from an OU\.

  ```
  $ aws organizations untag-resource \
      --resource-id ou-a1b2-f6g7h222 \
      --tag-keys Department
  ```

  This command produces no output when successful\.
+ AWS SDKs: [TagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_TagResource.html) and [UntagResource](https://docs.aws.amazon.com/organizations/latest/APIReference/API_UntagResource.html)

------

## Moving accounts to an OU or between the root and OUs<a name="move_account_to_ou"></a>

When you sign in to your organization's management account, you can move accounts in your organization from the root to an OU, from one OU to another, or back to the root from an OU\. Placing an account inside an OU makes it subject to any policies that are attached to the parent OU and any OUs in the parent chain up to the root\. If an account isn't in an OU, it's subject to only the policies that are attached directly to the root and any policies that are attached directly to the account\. To move accounts, complete the following steps\.

**Minimum permissions**  
To move accounts to a new location in the OU hierarchy, you must have the following permissions:  
`organizations:DescribeOrganization` – required only when using the Organizations console
`organizations:MoveAccount`

------
#### [ AWS Management Console ]

**To move accounts to an OU**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, find the account or accounts that you want to move\. You can navigate the OU hierarchy or enable **View AWS accounts only** to see a flat list of accounts without the OU structure\. If you have a lot of accounts, you might have to choose **Load more accounts in '*ou\-name*'** at the bottom of the list to find all of those you want to move\.

1. Choose the check box ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/checkbox-selected.png) next to the name of each account that you want to move\.

1. On the **Actions** menu, under **AWS account**, choose **Move **\.

1. In the **Move AWS account** dialog box, navigate to and then choose the OU or root that you want to move the account to, and then choose **Move AWS account**\.

------
#### [ AWS CLI & AWS SDKs ]

**To move an account to an OU**  
You can use one of the following commands to move an account:
+ AWS CLI: [move\-account](https://docs.aws.amazon.com/cli/latest/reference/organizations/move-account.html)

  The following example moves an AWS account from the root to an OU\. Note that you must specify the IDs of both the source and destination containers\.

  ```
  $ aws organizations move-account \
      --account-id 111122223333 \
      --source-parent-id r-a1b2 \
      --destination-parent-id ou-a1b2-f6g7h111
  ```

  This command produces no output when successful\.
+ AWS SDKs: [MoveAccount](https://docs.aws.amazon.com/organizations/latest/APIReference/API_MoveAccount.html)

------

## Deleting OUs<a name="delete-ou"></a>

When you sign in to your organization's management account, you can delete any OUs that you no longer need\. 

You must first move all accounts out of the OU and any child OUs, and then you can delete the child OUs\.

**Minimum permissions**  
To delete an OU, you must have the following permissions:  
`organizations:DescribeOrganization` – required only when using the Organizations console
`organizations:DeleteOrganizationalUnit`

------
#### [ AWS Management Console ]

**To delete an OU**

1. Sign in to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2)\. You must sign in as an IAM user, assume an IAM role, or sign in as the root user \([not recommended](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials)\) in the organization’s management account\.

1. On the **[AWS accounts](https://console.aws.amazon.com/organizations/v2/home/accounts)** page, find the OUs that you want to delete and choose the check box ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/organizations/latest/userguide/images/checkbox-selected.png) next to each OU's name\.

1. Choose **Actions**, and then under **Organizational unit**, choose **Delete**\.

1. To confirm that you want to delete the OUs, enter the OU's name \(if you chose to delete only one\) or the word 'delete' \(if you chose more than one\), and then choose **Delete**\.

   AWS Organizations deletes the OUs and removes them from the list\.

------
#### [ AWS CLI & AWS SDKs ]

**To delete an OU**  
You can use one of the following commands to delete an OU: 
+ AWS CLI: [delete\-organizational\-unit](https://docs.aws.amazon.com/cli/latest/reference/organizations/delete-organizational-unit.html)

  The following example shows how to delete an OU\.

  ```
  $ aws organizations delete-organizational-unit \
      --organizational-unit-id ou-a1b2-f6g7h222
  ```

  This command produces no output when successful\.
+ AWS SDKs: [DeleteOrganizationalUnit](https://docs.aws.amazon.com/organizations/latest/APIReference/API_DeleteOrganizationalUnit.html)

------