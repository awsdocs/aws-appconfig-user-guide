# Configuring permissions for AWS AppConfig<a name="getting-started-with-appconfig-permissions"></a>

By default, only AWS account adminstrators have access to AWS AppConfig\. You can grant AWS Identity and Access Management \(IAM\) users, groups, and roles access to AWS AppConfig by specifying resources, actions, and condition context keys in an IAM permission policy that you assign to the user, group, or role\. For more information, see [Actions, resources, and condition keys for AWS AppConfig](https://docs.aws.amazon.com/service-authorization/latest/reference/list_awsappconfig.html#awsappconfig-policy-keys) in the *Service Authorization Reference*\. 

**Important**  
Security is a shared responsibility between AWS and you\. The [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) describes this as security *of* the cloud and security *in* the cloud\. To understand how to apply the shared responsibility model when using AWS AppConfig, see to [Security in AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/security.html) in the *AWS Systems Manager User Guide*\. AWS AppConfig is a capability of AWS Systems Manager\.

We recommend that you create restrictive IAM permissions policies that grant users, groups, and roles the least privileges necessary to perform a desired action in AWS AppConfig\.

For example, you can create a read\-only IAM permissions policy that includes only the `Get` and `List` API actions used by AWS AppConfig, like the following\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ssm:GetDocument",
        "ssm:ListDocuments",
        "appconfig:ListApplications",
        "appconfig:GetApplication",
        "appconfig:ListEnvironments",
        "appconfig:GetEnvironment",
        "appconfig:ListConfigurationProfiles",
        "appconfig:GetConfigurationProfile",
        "appconfig:ListDeploymentStrategies",
        "appconfig:GetDeploymentStrategy",
        "appconfig:GetConfiguration",
        "appconfig:ListDeployments",
        "appconfig:GetDeployment"
               
      ],
      "Resource": "*"
    }
  ]
}
```

**Important**  
Restrict access to the [StartDeployment](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_StartDeployment.html) and [StopDeployment](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_StopDeployment.html) API actions to trusted users who understand the responsibilities and consequences of deploying a new configuration to your targets\.

For more information about creating and editing IAM policies, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\. For information about how to assign this policy to an IAM group, see [Attaching a Policy to an IAM Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_attach-policy.html)\. 

**To configure an IAM user account with permission to use AWS AppConfig**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**\.

1. In the list, choose a name\.

1. Choose the **Permissions** tab\.

1. On the right side of the page, under **Permission policies**, choose **Add inline policy**\. 

1. Choose the **JSON** tab\.

1. Replace the default content with your custom permissions policy\.

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy\. For example: `AWSAppConfig-<action>-Access`\.

1. Choose **Create policy**\.