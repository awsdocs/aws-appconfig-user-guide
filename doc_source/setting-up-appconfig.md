# Setting up AWS AppConfig<a name="setting-up-appconfig"></a>

If you haven't already done so, sign up for an AWS account and create an administrative user\.

## Sign up for an AWS account<a name="sign-up-for-aws"></a>

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

   When you sign up for an AWS account, an *AWS account root user* is created\. The root user has access to all AWS services and resources in the account\. As a security best practice, [assign administrative access to an administrative user](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html), and use only the root user to perform [tasks that require root user access](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html)\.

AWS sends you a confirmation email after the sign\-up process is complete\. At any time, you can view your current account activity and manage your account by going to [https://aws\.amazon\.com/](https://aws.amazon.com/) and choosing **My Account**\.

## Create an administrative user<a name="create-an-admin"></a>

After you sign up for an AWS account, create an administrative user so that you don't use the root user for everyday tasks\.

**Secure your AWS account root user**

1.  Sign in to the [AWS Management Console](https://console.aws.amazon.com/) as the account owner by choosing **Root user** and entering your AWS account email address\. On the next page, enter your password\.

   For help signing in by using root user, see [Signing in as the root user](https://docs.aws.amazon.com/signin/latest/userguide/console-sign-in-tutorials.html#introduction-to-root-user-sign-in-tutorial) in the *AWS Sign\-In User Guide*\.

1. Turn on multi\-factor authentication \(MFA\) for your root user\.

   For instructions, see [Enable a virtual MFA device for your AWS account root user \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html#enable-virt-mfa-for-root) in the *IAM User Guide*\.

**Create an administrative user**
+ For your daily administrative tasks, grant administrative access to an administrative user in AWS IAM Identity Center \(successor to AWS Single Sign\-On\)\.

  For instructions, see [Getting started](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide*\.

**Sign in as the administrative user**
+ To sign in with your IAM Identity Center user, use the sign\-in URL that was sent to your email address when you created the IAM Identity Center user\.

  For help signing in using an IAM Identity Center user, see [Signing in to the AWS access portal](https://docs.aws.amazon.com/signin/latest/userguide/iam-id-center-sign-in-tutorial.html) in the *AWS Sign\-In User Guide*\.

## Grant programmatic access<a name="setting-up-appconfig-programmatic-access"></a>

Users need programmatic access if they want to interact with AWS outside of the AWS Management Console\. The way to grant programmatic access depends on the type of user that's accessing AWS:
+ If you manage identities in IAM Identity Center, the AWS APIs require a profile, and the AWS Command Line Interface requires a profile or an environment variable\.
+ If you have IAM users, the AWS APIs and the AWS Command Line Interface require access keys\. Whenever possible, create temporary credentials that consist of an access key ID, a secret access key, and a security token that indicates when the credentials expire\.

To grant users programmatic access, choose one of the following options\.


****  

| Which user needs programmatic access? | To | By | 
| --- | --- | --- | 
|  Workforce identity \(Users managed in IAM Identity Center\)  | Use short\-term credentials to sign programmatic requests to the AWS CLI or AWS APIs \(directly or by using the AWS SDKs\)\. |  Following the instructions for the interface that you want to use: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/appconfig/latest/userguide/setting-up-appconfig.html)  | 
| IAM | Use short\-term credentials to sign programmatic requests to the AWS CLI or AWS APIs \(directly or by using the AWS SDKs\)\. | Following the instructions in [Using temporary credentials with AWS resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html) in the IAM User Guide\. | 
| IAM | Use long\-term credentials to sign programmatic requests to the AWS CLI or AWS APIs \(directly or by using the AWS SDKs\)\.\(Not recommended\) | Following the instructions in [Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the IAM User Guide\. | 

## \(Optional\) Configure permissions for rollback based on CloudWatch alarms<a name="getting-started-with-appconfig-cloudwatch-alarms-permissions"></a>

You can configure AWS AppConfig to roll back to a previous version of a configuration in response to one or more Amazon CloudWatch alarms\. When you configure a deployment to respond to CloudWatch alarms, you specify an AWS Identity and Access Management \(IAM\) role\. AWS AppConfig requires this role so that it can monitor CloudWatch alarms\.

**Note**  
The IAM role must belong to the current account\. By default, AWS AppConfig can only monitor alarms owned by the current account\. If you want to configure AWS AppConfig to roll back deployments in response to metrics from a different account, you must configure cross account alarms\. For more information, see [Cross\-account cross\-Region CloudWatch console](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Cross-Account-Cross-Region.html) in the *Amazon CloudWatch User Guide*\.

Use the following procedures to create an IAM role that enables AWS AppConfig to rollback based on CloudWatch alarms\. This section includes the following procedures\.

1. [Step 1: Create the permission policy for rollback based on CloudWatch alarms](#getting-started-with-appconfig-cloudwatch-alarms-permissions-policy)

1. [Step 2: Create the IAM role for rollback based on CloudWatch alarms](#getting-started-with-appconfig-cloudwatch-alarms-permissions-role)

1. [Step 3: Add a trust relationship](#getting-started-with-appconfig-cloudwatch-alarms-permissions-trust)

### Step 1: Create the permission policy for rollback based on CloudWatch alarms<a name="getting-started-with-appconfig-cloudwatch-alarms-permissions-policy"></a>

Use the following procedure to create an IAM policy that gives AWS AppConfig permission to call the `DescribeAlarms` API action\. 

**To create an IAM permission policy for rollback based on CloudWatch alarms**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. On the **Create policy** page, choose the **JSON** tab\.

1. Replace the default content on the JSON tab with the following permission policy, and then choose **Next: Tags**\.

   ```
   {
           "Version": "2012-10-17",
           "Statement": [
               {
                   "Effect": "Allow",
                   "Action": [
                       "cloudwatch:DescribeAlarms"
                   ],
                   "Resource": "*"
               }
           ]
       }
   ```

1. Enter tags for this role, and then choose **Next: Review**\.

1. On the **Review** page, enter **SSMCloudWatchAlarmDiscoveryPolicy** in the **Name** field\. 

1. Choose **Create policy**\. The system returns you to the **Policies** page\.

### Step 2: Create the IAM role for rollback based on CloudWatch alarms<a name="getting-started-with-appconfig-cloudwatch-alarms-permissions-role"></a>

Use the following procedure to create an IAM role and assign the policy you created in the previous procedure to it\. 

**To create an IAM role for rollback based on CloudWatch alarms**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. Under **Select type of trusted entity**, choose **AWS service**\.

1. Immediately under **Choose the service that will use this role**, choose **EC2: Allows EC2 instances to call AWS services on your behalf**, and then choose **Next: Permissions**\.

1. On the **Attached permissions policy** page, search for **SSMCloudWatchAlarmDiscoveryPolicy**\. 

1. Choose this policy and then choose **Next: Tags**\.

1. Enter tags for this role, and then choose **Next: Review**\.

1. On the **Create role** page, enter **SSMCloudWatchAlarmDiscoveryRole** in the **Role name** field, and then choose **Create role**\.

1. On the **Roles** page, choose the role you just created\. The **Summary** page opens\. 

### Step 3: Add a trust relationship<a name="getting-started-with-appconfig-cloudwatch-alarms-permissions-trust"></a>

Use the following procedure to configure the role you just created to trust AWS AppConfig\.

**To add a trust relationship for AWS AppConfig**

1. In the **Summary** page for the role you just created, choose the **Trust Relationships** tab, and then choose **Edit Trust Relationship**\.

1. Edit the policy to include only "`appconfig.amazonaws.com`", as shown in the following example:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "appconfig.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Choose **Update Trust Policy**\.