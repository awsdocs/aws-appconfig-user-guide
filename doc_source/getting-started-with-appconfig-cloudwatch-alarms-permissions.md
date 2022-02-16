# \(Optional\) Configuring permissions for rollback based on CloudWatch alarms<a name="getting-started-with-appconfig-cloudwatch-alarms-permissions"></a>

You can configure AWS AppConfig to roll back to a previous version of a configuration in response to one or more Amazon CloudWatch alarms\. When you configure a deployment to respond to CloudWatch alarms, you specify an AWS Identity and Access Management \(IAM\) role\. AWS AppConfig requires this role so that it can monitor CloudWatch alarms\.

**Note**  
The IAM role must belong to the current account\. By default, AWS AppConfig can only monitor alarms owned by the current account\. If you want to configure AWS AppConfig to roll back deployments in response to metrics from a different account, you must configure cross account alarms\. For more information, see [Cross\-account cross\-Region CloudWatch console](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Cross-Account-Cross-Region.html) in the *Amazon CloudWatch User Guide*\.

Use the following procedures to create an IAM role that enables AWS AppConfig to rollback based on CloudWatch alarms\. This section includes the following procedures\.

1. [Step 1: Create the permission policy for rollback based on CloudWatch alarms](#getting-started-with-appconfig-cloudwatch-alarms-permissions-policy)

1. [Step 2: Create the IAM role for rollback based on CloudWatch alarms](#getting-started-with-appconfig-cloudwatch-alarms-permissions-role)

1. [Step 3: Add a trust relationship](#getting-started-with-appconfig-cloudwatch-alarms-permissions-trust)

## Step 1: Create the permission policy for rollback based on CloudWatch alarms<a name="getting-started-with-appconfig-cloudwatch-alarms-permissions-policy"></a>

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

## Step 2: Create the IAM role for rollback based on CloudWatch alarms<a name="getting-started-with-appconfig-cloudwatch-alarms-permissions-role"></a>

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

## Step 3: Add a trust relationship<a name="getting-started-with-appconfig-cloudwatch-alarms-permissions-trust"></a>

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