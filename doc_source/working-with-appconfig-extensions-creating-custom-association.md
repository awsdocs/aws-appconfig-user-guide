# Creating an extension association for a custom AWS AppConfig extension<a name="working-with-appconfig-extensions-creating-custom-association"></a>

To create an extension, or configure an AWS authored extension, you define the action points that invoke an extension when a specific AWS AppConfig resource is used\. For example, you can choose to run the `AWS AppConfig deployment events to Amazon SNS` extension and receive notifications on an Amazon SNS topic anytime a configuration deployment is started for a specific application\. Defining which action points invoke an extension for a specific AWS AppConfig resource is called an *extension association*\. An extension association is a specified relationship between an extension and an AWS AppConfig resource, such as an application or a configuration profile\.

A single AWS AppConfig application can include multiple environments and configuration profiles\. If you associate an extension to an application or an environment, AWS AppConfig invokes the extension for any workflows that relate to the application or environment resources, if applicable\.

For example, say you have an AWS AppConfig application called MobileApps that includes a configuration profile called AccessList\. And say the MobileApps application includes Beta, Integration, and Production environments\. You create an extension association for the AWS authored Amazon SNS notification extension and associate the extension to the MobileApps application\. The Amazon SNS notification extension is invoked anytime the configuration is deployed for the application to any of the three environments\. 

Use the following procedures to create an AWS AppConfig extension association by using the AWS AppConfig console\.

**To create an extension association \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/appconfig/](https://console.aws.amazon.com/systems-manager/appconfig/)\.

1. In the navigation pane, choose **AWS AppConfig**\.

1. On the **Extensions** tab, choose an option button for an extension and then choose **Add to resource**\. For the purposes of this walkthrough, choose **MyS3ConfigurationBackUpExtension**\.

1. In the **Extension resource details** section, for **Resource type**, choose an AWS AppConfig resource type\. Depending on the resource you choose, AWS AppConfig prompts you to choose other resources\. For the purposes of this walkthrough, choose **Application**\.

1. Choose an application in the list\.

1. In the **Parameters** section, verify that **S3\_BUCKET** is listed in the **Key** field\. In the **Value** field, paste the ARN of the Lambda extensions\. For example: `arn:aws:lambda:aws-region:111122223333:function:MyS3ConfigurationBackUpExtension`\.

1. Choose **Create association to resource**\.