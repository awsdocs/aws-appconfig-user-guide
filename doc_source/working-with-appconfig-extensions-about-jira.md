# Working with the Atlassian Jira extension for AWS AppConfig<a name="working-with-appconfig-extensions-about-jira"></a>

Integrating with Atlassian Jira allows AWS AppConfig to create and update issues in the Atlassian console whenever you make changes to a [feature flag](https://docs.aws.amazon.com/appconfig/latest/userguide/appconfig-creating-configuration-and-profile.html#appconfig-creating-configuration-and-profile-feature-flags) in your AWS account for the specified AWS Region\. Each Jira issue includes the flag name, application ID, configuration profile ID, and flag values\. After you update, save, and deploy your flag changes, Jira updates the existing issues with the details of the change\.

**Note**  
Jira updates issues whenever you create or update a feature flag\. Jira also updates issues when you delete a child\-level flag attribute from a parent\-level flag\. Jira does not record information when you delete a parent\-level flag\.

To configure integration, you must do the following:
+ [Configure permissions](https://docs.aws.amazon.com/appconfig/latest/userguide/working-with-appconfig-extensions-about-jira-permissions.html)
+ [Configure integration](https://docs.aws.amazon.com/appconfig/latest/userguide/working-with-appconfig-extensions-about-jira-configure.html)

## Configuring permissions for AWS AppConfig Jira integration<a name="working-with-appconfig-extensions-about-jira-permissions"></a>

When you configure AWS AppConfig integration with Jira, you specify credentials for an AWS Identity and Access Management \(IAM\) user\. Specifically, you enter the user's access key ID and secret key in the AWS AppConfig for Jira application\. This user gives Jira permission to communicate with AWS AppConfig\. AWS AppConfig uses these credentials one time to establish an association between AWS AppConfig and Jira\. The credentials are not stored\. You can remove the association by uninstalling the AWS AppConfig for Jira application\.

The IAM user account requires a permissions policy that includes the following actions:
+ `appconfig:CreateExtensionAssociation`
+ `appconfig:GetConfigurationProfile`
+ `appconfig:ListApplications`
+ `appconfig:ListConfigurationProfiles`
+ `appconfig:ListExtensionAssociations`
+ `sts:GetCallerIdentity`

Complete the following tasks to create an IAM permissions policy and an IAM user for AWS AppConfig and Jira integration:

**Tasks**
+ [Task 1: Create an IAM permissions policy for AWS AppConfig and Jira integration](#working-with-appconfig-extensions-about-jira-permissions-policy)
+ [Task 2: Create an IAM user for AWS AppConfig and Jira integration](#working-with-appconfig-extensions-about-jira-permissions-user)

### Task 1: Create an IAM permissions policy for AWS AppConfig and Jira integration<a name="working-with-appconfig-extensions-about-jira-permissions-policy"></a>

Use the following procedure to create an IAM permissions policy that allows Atlassian Jira to communicate with AWS AppConfig\. We recommend that you create a new policy and attach this policy to a new IAM role\. Adding the required permissions to an existing IAM policy and role goes against the principle of least privilege and is not recommended\. 

**To create an IAM policy for AWS AppConfig and Jira integration**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. On the **Create policy** page, choose the **JSON** tab and replace the default content with the following policy\. In the following policy, replace *Region*, *account\_ID*, *application\_ID*, and *configuration\_profile\_ID* with information from your AWS AppConfig feature flag environment\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                          "appconfig:AssociateExtension",
                          "appconfig:GetConfigurationProfile",
                          "appconfig:UpdateConfigurationProfile"
                          
                ],
               "Resource": [
                          "arn:aws:appconfig:Region:account_ID:application/application_ID",
                          "arn:aws:appconfig:Region:account_ID:application/application_ID/configurationprofile/configuration_profile_ID"
                ]
           },
          {
               "Effect": "Allow",
               "Action": [
                          "appconfig:ListApplications"
                          
                ],
               "Resource": [
                          "arn:aws:appconfig:Region:account_ID:*"
                ]
           },
            {
               "Effect": "Allow",
               "Action": [
                          "appconfig:ListConfigurationProfiles"
                ],
               "Resource": [
                          "arn:aws:appconfig:Region:account_ID:application/application_ID"
                ]
           },
           {
               "Effect": "Allow",
               "Action": "sts:GetCallerIdentity",
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Next: Tags**\.

1. \(Optional\) Add one or more tag\-key value pairs to organize, track, or control access for this policy, and then choose **Next: Review**\. 

1. On the **Review policy** page, enter a name in the **Name** box, such as **AppConfigJiraPolicy**, and then enter an optional description\.

1. Choose **Create policy**\.

### Task 2: Create an IAM user for AWS AppConfig and Jira integration<a name="working-with-appconfig-extensions-about-jira-permissions-user"></a>

Use the following procedure to create an IAM user for AWS AppConfig and Atlassian Jira integration\. After you create the user, you can copy the access key ID and secret key, which you will specify when you complete the integration\.

**To create an IAM user for AWS AppConfig and Jira integration**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**, and then choose **Add users**\.

1. In the **User name** field, enter a name, such as **AppConfigJiraUser**\.

1. For **Select AWS credential type**, choose **Access key \- Programmatic access**\.

1. Choose **Next: Permissions**\.

1. Under **Set permissions** page, choose **Attach existing policies directly**\. Search for and select the check box for the policy that you created in [Task 1: Create an IAM permissions policy for AWS AppConfig and Jira integration](#working-with-appconfig-extensions-about-jira-permissions-policy), and then choose **Next: Tags**\.

1. On the **Add tags \(optional\)** page, add one or more tag\-key value pairs to organize, track, or control access for this user\. Choose **Next: Review**\.

1. On the **Review** page, verify the user details\.

1. Choose **Create user**\. The system displays the user's access key ID and secret key\. Either download the \.csv file or copy these credentials to a separate location\. You will specify these credentials when you configure integration\.

## Configuring the AWS AppConfig Jira integration application<a name="working-with-appconfig-extensions-about-jira-configure"></a>

Use the following procedure to configure required options in the AWS AppConfig for Jira application\. After you complete this procedure, Jira creates a new issue for each feature flag in your AWS account for the specified AWS Region\. If you make changes to a feature flag in AWS AppConfig, Jira records the details in the existing issues\.

**Note**  
An AWS AppConfig feature flag can include multiple child\-level flag attributes\. Jira creates one issue for each parent\-level feature flag\. If you change a child\-level flag attribute, you can view the details of that change in the Jira issue for the parent\-level flag\.

**To configure integration**

1. Log in to the [Atlassian Marketplace](https://marketplace.atlassian.com/)\.

1. Type **AWS AppConfig** in the search field and press **Enter**\.

1. Install the application on your Jira instance\.

1. In the Atlassian console, choose **Manage apps**, and then choose **AWS AppConfig for Jira**\.

1. Choose **Configure**\.

1. Under **Configuration details**, choose **Jira project** and then choose the project that you want to associate with your AWS AppConfig feature flag\.

1. Choose **AWS Region**, and then choose the Region where your AWS AppConfig feature flag is located\.

1. In the **Application ID** field, enter the name of the AWS AppConfig application that contains your feature flag\.

1. In the **Configuration profile ID** field, enter the name of the AWS AppConfig configuration profile for your feature flag\.

1. In the **Access key ID** and **Secret key** fields, enter the credentials you copied in [Task 2: Create an IAM user for AWS AppConfig and Jira integration](#working-with-appconfig-extensions-about-jira-permissions-user)\. Optionally, you can also specify a session token\.

1. Choose **Submit**\.

1. In the Atlassian console, choose **Projects**, and then choose the project you selected for AWS AppConfig integration\. The **Issues** page displays an issue for each feature flag in the specified AWS account and AWS Region\.

## Deleting the AWS AppConfig for Jira application and data<a name="working-with-appconfig-extensions-about-jira-delete"></a>

If you no longer want to use Jira integration with AWS AppConfig feature flags, you can delete the AWS AppConfig for Jira application in the Atlassian console\. Deleting the integration application does the following:
+ Deletes the association between your Jira instance and AWS AppConfig\.
+ Deletes your Jira instance details from AWS AppConfig\.

**To delete the AWS AppConfig for Jira application**

1. In the Atlassian console, choose **Manage apps**\.

1. Choose **AWS AppConfig for Jira**\.

1. Choose **Uninstall**\.