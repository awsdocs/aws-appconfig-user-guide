# Working with the `AWS AppConfig deployment events to Amazon SQS` extension<a name="working-with-appconfig-extensions-about-predefined-notification-sqs"></a>

The `AWS AppConfig deployment events to Amazon SQS` extension is an AWS authored extension that helps you monitor and act on the AWS AppConfig configuration deployment workflow\. The extension enqueues messages into your Amazon Simple Queue Service \(Amazon SQS\) queue whenever a configuration is deployed\. After you associate the extension to one of your AWS AppConfig applications, environments, or configuration profiles, AWS AppConfig enqueues a message into the queue after every configuration deployment start, end, and rollback\.

If you want more control over which action points send Amazon SQS notifications, you can create a custom extension and enter an Amazon SQS queue Amazon Resource Name \(ARN\) for the URI field\. For information about creating an extension, see [Walkthrough: Creating custom AWS AppConfig extensions](working-with-appconfig-extensions-creating-custom.md)\.

## Using the extension<a name="working-with-appconfig-extensions-about-predefined-notification-sqs-using"></a>

This section describes how to use the `AWS AppConfig deployment events to Amazon SQS` extension\.

**Step 1: Configure AWS AppConfig to enqueue messages**  
Add an Amazon SQS policy to your Amazon SQS queue granting AWS AppConfig \(`appconfig.amazonaws.com`\) send message permissions \(`sqs:SendMessage`\)\. For more information, see [Basic examples of Amazon SQS policies](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-basic-examples-of-sqs-policies.html)\.

**Step 2: Create an extension association**  
Attach the extension to one of your AWS AppConfig resources by creating an extension association\. You create the association by using the AWS AppConfig console or the [CreateExtensionAssociation](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_CreateExtensionAssociation.html) API action\. When you create the association, you specify the ARN of an AWS AppConfig application, environment, or configuration profile\. If you associate the extension to an application or an environment, a notification is sent for any configuration profile contained within the specified application or environment\. When you create the association, you must enter a `Here` parameter that contains the ARN of the Amazon SQS queue you want to use\.

After you create the association, when a configuration for the specified AWS AppConfig resource is created or deployed, AWS AppConfig invokes the extension and sends notifications according to the action points specified in the extension\.

**Note**  
This extension is invoked by the following action points:  
`ON_DEPLOYMENT_START`
`ON_DEPLOYMENT_COMPLETE`
`ON_DEPLOYMENT_ROLLED_BACK`
You can't customize the actions points for this extension\. To invoke different action points, you can create your own extension\. For more information, see [Walkthrough: Creating custom AWS AppConfig extensions](working-with-appconfig-extensions-creating-custom.md)\.

Use the following procedures to create an AWS AppConfig extension association by using either the AWS Systems Manager console or the AWS CLI\.

**To create an extension association \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/appconfig/](https://console.aws.amazon.com/systems-manager/appconfig/)\.

1. In the navigation pane, choose **AWS AppConfig**\.

1. On the **Extensions** tab, choose **Add to resource**\.

1. In the **Extension resource details** section, for **Resource type**, choose an AWS AppConfig resource type\. Depending on the resource you choose, AWS AppConfig prompts you to choose other resources\.

1. Choose **Create association to resource**\.

Here's an example of the message sent to the Amazon SQS queue when the extension is invoked\.

```
{
   "InvocationId":"7itcaxp",
   "Parameters":{
      "queueArn":"arn:aws:sqs:us-east-1:111122223333:MySQSQueue"
   },
   "Application":{
      "Id":"1a2b3c4d",
      "Name":MyApp
   },
   "Environment":{
      "Id":"1a2b3c4d",
      "Name":MyEnv
   },
   "ConfigurationProfile":{
      "Id":"1a2b3c4d",
      "Name":"MyConfigProfile"
   },
   "Description":null,
   "DeploymentNumber":"3",
   "ConfigurationVersion":"1",
   "Type":"OnDeploymentComplete"
}
```