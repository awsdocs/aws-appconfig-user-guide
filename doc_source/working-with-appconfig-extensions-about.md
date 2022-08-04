# About AWS AppConfig extensions<a name="working-with-appconfig-extensions-about"></a>

This topic introduces AWS AppConfig extension concepts and terminology\. The information is discussed in the context of each step required to set up and use AWS AppConfig extensions\.

**Topics**
+ [Step 1: Determine what you want to do with extensions](#working-with-appconfig-extensions-how-it-works-step-1)
+ [Step 2: Determine when you want the extension to run](#working-with-appconfig-extensions-how-it-works-step-2)
+ [Step 3: Create an extension association](#working-with-appconfig-extensions-how-it-works-step-3)
+ [Step 4: Deploy a configuration and verify the extension actions are performed](#working-with-appconfig-extensions-how-it-works-step-4)

## Step 1: Determine what you want to do with extensions<a name="working-with-appconfig-extensions-how-it-works-step-1"></a>

Do you want to receive a notification to a webhook that sends messages to Slack anytime an AWS AppConfig deployment completes? Do you want to back up a configuration profile to an Amazon Simple Storage Service \(Amazon S3\) bucket before a configuration is deployed? Do you want to scrub configuration data for sensitive information before the configuration is deployed? You can use extensions to perform these types of tasks and more\. You can create custom extensions or use the AWS authored extensions included with AWS AppConfig\.

**Note**  
For most use cases, to create a custom extension, you must create an AWS Lambda function to perform any computation and processing defined in the extension\. For more information, see [Walkthrough: Creating custom AWS AppConfig extensions](working-with-appconfig-extensions-creating-custom.md)\.

The following AWS authored extensions can help you quickly integrate configuration deployments with other services\. You can use these extensions in the AWS AppConfig console\.


****  

| Extension | Description | 
| --- | --- | 
|  [AWS AppConfig deployment events to EventBridge](https://docs.aws.amazon.com/appconfig/latest/userguide/working-with-appconfig-extensions-about-predefined-notification-eventbridge.html)  |  This extension sends events to the EventBridge default event bus when a configuration is deployed\.   | 
|  [AWS AppConfig deployment events to Amazon Simple Notification Service \(Amazon SNS\)](https://docs.aws.amazon.com/appconfig/latest/userguide/working-with-appconfig-extensions-about-predefined-notification-sns.html)  |  This extension sends messages to an Amazon SNS topic that you specify when a configuration is deployed\.   | 
|  [AWS AppConfig deployment events to Amazon Simple Queue Service \(Amazon SQS\)](https://docs.aws.amazon.com/appconfig/latest/userguide/working-with-appconfig-extensions-about-predefined-notification-sqs.html)  |  This extension enqueues messages into your Amazon SQS queue when a configuration is deployed\.  | 
|  [Integration extension—Atlassian Jira](https://docs.aws.amazon.com/appconfig/latest/userguide/working-with-appconfig-extensions-about-jira.html)  |  This extensions allows AWS AppConfig to create and update issues whenever you make changes to a [feature flag](https://docs.aws.amazon.com/appconfig/latest/userguide/appconfig-creating-configuration-and-profile.html#appconfig-creating-configuration-and-profile-feature-flags)\.   | 

## Step 2: Determine when you want the extension to run<a name="working-with-appconfig-extensions-how-it-works-step-2"></a>

An extension defines one or more actions that it performs during an AWS AppConfig workflow\. For example, the AWS authored `AWS AppConfig deployment events to Amazon SNS` extension includes an action to send a notification to an Amazon SNS topic\. Each action is invoked either when you interact with AWS AppConfig or when AWS AppConfig is performing a process on your behalf\. These are called *action points*\. AWS AppConfig extensions support the following action points:
+ `PRE_CREATE_HOSTED_CONFIGURATION_VERSION`
+ `PRE_START_DEPLOYMENT`
+ `ON_DEPLOYMENT_START`
+ `ON_DEPLOYMENT_STEP`
+ `ON_DEPLOYMENT_BAKING`
+ `ON_DEPLOYMENT_COMPLETE`
+ `ON_DEPLOYMENT_ROLLED_BACK`

Extension actions configured on `PRE_*` action points are applied after request validation, but before AWS AppConfig performs the activity that corresponds to the action point name\. These action invocations are processed at the same time as a request\. If more than one request is made, action invocations run sequentially\. Also note that `PRE_*` action points receive and can change the contents of a configuration\. `PRE_*` action points can also respond to an error and prevent an action from happening\. 

An extension can also run in parallel with an AWS AppConfig workflow by using an `ON_*` action point\. `ON_*` action points are invoked asynchronously\. `ON_*` action points don't receive the contents of a configuration\. If an extension experiences an error during an `ON_*` action point, the service ignores the error and continues the workflow\.

## Step 3: Create an extension association<a name="working-with-appconfig-extensions-how-it-works-step-3"></a>

To create an extension, or configure an AWS authored extension, you define the action points that invoke an extension when a specific AWS AppConfig resource is used\. For example, you can choose to run the `AWS AppConfig deployment events to Amazon SNS` extension and receive notifications on an Amazon SNS topic anytime a configuration deployment is started for a specific application\. Defining which action points invoke an extension for a specific AWS AppConfig resource is called an *extension association*\. An extension association is a specified relationship between an extension and an AWS AppConfig resource, such as an application or a configuration profile\.

A single AWS AppConfig application can include multiple environments and configuration profiles\. If you associate an extension to an application or an environment, AWS AppConfig invokes the extension for any workflows that relate to the application or environment resources, if applicable\.

For example, say you have an AWS AppConfig application called MobileApps that includes a configuration profile called AccessList\. And say the MobileApps application includes Beta, Integration, and Production environments\. You create an extension association for the AWS authored Amazon SNS notification extension and associate the extension to the MobileApps application\. The Amazon SNS notification extension is invoked anytime the configuration is deployed for the application to any of the three environments\. 

**Note**  
You don't have to create an extension to use AWS authored extensions, but you do have to create an extension association\.

## Step 4: Deploy a configuration and verify the extension actions are performed<a name="working-with-appconfig-extensions-how-it-works-step-4"></a>

After you create an association, when a hosted configuration is created or a configuration is deployed, AWS AppConfig invokes the extension and performs the specified actions\. When an extension is invoked, if the system experiences an error during a `PRE-*` action point, AWS AppConfig returns information about that error\.