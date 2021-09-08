# Configuring permissions for AWS AppConfig<a name="getting-started-with-appconfig-permissions"></a>

AWS AppConfig uses the following API actions\.


****  

| AWS AppConfig resource | API actions | 
| --- | --- | 
|  AWS AppConfig applications  | [CreateApplication](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_CreateApplication.html), [UpdateApplication](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_UpdateApplication.html), [DeleteApplication](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_DeleteApplication.html), [GetApplication](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetApplication.html), [ListApplications](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_ListApplications.html)  For more information, see [Step 1: Creating an AWS AppConfig application ](appconfig-creating-application.md)  | 
|  Environments  | [CreateEnvironment](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_CreateEnvironment.html), [UpdateEnvironment](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_UpdateEnvironment.html), [DeleteEnvironment](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_DeleteEnvironment.html), [GetEnvironment](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetEnvironment.html), [ListEnvironments](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_ListEnvironments.html)  For more information, see [Step 2: Creating an environment ](appconfig-creating-environment.md)  | 
|  Configurations  | [GetConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetConfiguration.html)  For more information, see [Step 3: Creating a configuration and a configuration profile ](appconfig-creating-configuration-and-profile.md)  | 
|  Configuration profiles  | [CreateConfigurationProfile](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_CreateConfigurationProfile.html), [UpdateConfigurationProfile](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_UpdateConfigurationProfile.html), [DeleteConfigurationProfile](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_DeleteConfigurationProfile.html), [GetConfigurationProfile](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetConfigurationProfile.html), [ValidateConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_ValidateConfiguration.html), [ListConfigurationProfiles](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_ListConfigurationProfiles.html), [GetHostedConfigurationVersion](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetHostedConfigurationVersion.html), [ListHostedConfigurationVersions](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_ListHostedConfigurationVersions.html) For more information, see [Step 3: Creating a configuration and a configuration profile ](appconfig-creating-configuration-and-profile.md)  | 
|  Deployment strategies  | [CreateDeploymentStrategy](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_CreateDeploymentStrategy.html), [UpdateDeploymentStrategy](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_UpdateDeploymentStrategy.html), [DeleteDeploymentStrategy](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_DeleteDeploymentStrategy.html), [GetDeploymentStrategy](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetDeploymentStrategy.html), [ListDeploymentStrategies](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_ListDeploymentStrategies.html), [CreateDocument](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateDocument.html), [UpdateDocument](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateDocument.html), [DeleteDocument](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteDocument.html), [GetDocument](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetDocument.html), [ListDocuments](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ListDocuments.html)  For more information, see [Step 4: Creating a deployment strategy](appconfig-creating-deployment-strategy.md)  | 
|  Deployments  | [StartDeployment](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_StartDeployment.html), [StopDeployment](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_StopDeployment.html), [ListDeployments](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_ListDeployments.html)  For more information, see [Step 5: Deploying a configuration](appconfig-deploying.md)  | 

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
        "appconfig:ListDeployments"
               
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