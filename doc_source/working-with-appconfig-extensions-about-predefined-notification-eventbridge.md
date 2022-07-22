# Working with the `AWS AppConfig deployment events to Amazon EventBridge` extension<a name="working-with-appconfig-extensions-about-predefined-notification-eventbridge"></a>

The `AWS AppConfig deployment events to Amazon EventBridge` extension is an AWS\-authored extension that helps you monitor and act on the AWS AppConfig configuration deployment workflow\. The extension sends event notifications to the EventBridge default events bus whenever a configuration is deployed\. After you’ve associated the extension to one of your AWS AppConfig applications, environments, or configuration profiles, AWS AppConfig sends event notifications to the event bus after every configuration deployment start, end, and rollback\.

If you want more control over which action points send EventBridge notifications, you can create a custom extension and enter the EventBridge default events bus Amazon Resource Name \(ARN\) for the URI field\. For information about creating an extension, see [Creating custom AWS AppConfig extensions](working-with-appconfig-extensions-creating-custom.md)\.

**Important**  
This extension supports only the EventBridge default events bus\.

## Using the extension<a name="working-with-appconfig-extensions-about-predefined-notification-ev-using"></a>

To use the `AWS AppConfig deployment events to Amazon EventBridge` extension, you first attach the extension to one of your AWS AppConfig resources by creating an extension association\. You create the association by using the AWS AppConfig console or the [CreateExtensionAssociation](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_CreateExtensionAssociation.html) API action\. When you create the association, you specify the ARN of an AWS AppConfig application, environment, or configuration profile\. If you associate the extension to an application or an environment, an event notification is sent for any configuration profile contained within the specified application or environment\.

After you create the association, when a configuration for the specified AWS AppConfig resource is deployed, AWS AppConfig invokes the extension and sends notifications according to the action points specified in the extension\.

**Note**  
This extension is invoked by the following action points\.  
`ON_DEPLOYMENT_START`
`ON_DEPLOYMENT_COMPLETE`
`ON_DEPLOYMENT_ROLLED_BACK`
You can't customize the actions points for this extension\. To invoke different action points, you can create your own extension\. For more information, see [Creating custom AWS AppConfig extensions](working-with-appconfig-extensions-creating-custom.md)\.

Use the following procedures to create an AWS AppConfig extension association by using either the AWS Systems Manager console or the AWS CLI\.

**To create an extension association \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/appconfig/](https://console.aws.amazon.com/systems-manager/appconfig/)\.

1. In the navigation pane choose **AWS AppConfig**\.

1. On the **Extensions** tab, choose **Add to resource**\.

1. In the **Extension resource details** section, for **Resource type**, choose an AWS AppConfig resource type\. Depending on the resource you choose, AWS AppConfig prompts you to choose other resources\.

1. Choose **Create association to resource**\.

Here's a sample event sent to EventBridge when the extension is invoked\.

```
{
   "version":"0",
   "id":"c53dbd72-c1a0-2302-9ed6-c076e9128277",
   "detail-type":"On Deployment Complete",
   "source":"aws.appconfig",
   "account":"111122223333",
   "time":"2022-07-09T01:44:15Z",
   "region":"us-east-1",
   "resources":[
      "arn:aws:appconfig:us-east-1:111122223333:extensionassociation/z763ff5"
   ],
   "detail":{
      "InvocationId":"5tfjcig",
       "Parameters":{
         
      },
      "Type":"OnDeploymentComplete",
      "Application":{
         "Id":"ba8toh7",
         "Name":"MyApp"
      },
      "Environment":{
         "Id":"pgil2o7",
         "Name":"MyEnv"
      },
      "ConfigurationProfile":{
         "Id":"ga3tqep",
         "Name":"MyConfigProfile"
      },
      "DeploymentNumber":1,
      "ConfigurationVersion":"1"
   }
}
```