# Working with AWS authored extensions<a name="working-with-appconfig-extensions-about-predefined"></a>

AWS AppConfig includes the following AWS authored extensions\. These extensions can help you integrate the AWS AppConfig workflow with other services\. You can use these extensions in the AWS Management Console or by calling extension [API actions](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_Operations.html) directly from the AWS CLI, AWS Tools for PowerShell, or the SDK\.


****  

| Extension | Description | 
| --- | --- | 
|  [Amazon CloudWatch Evidently A/B testing](https://docs.aws.amazon.com/appconfig/latest/userguide/working-with-appconfig-extensions-about-predefined-evidently)  |  This extension allows your application to assign variations to user sessions locally instead of by calling the [EvaluateFeature](https://docs.aws.amazon.com/cloudwatchevidently/latest/APIReference/API_EvaluateFeature.html) operation\. For more information, see [Working with the Amazon CloudWatch Evidently extension](working-with-appconfig-extensions-about-predefined-evidently.md)\.  | 
|  [AWS AppConfig deployment events to EventBridge](https://docs.aws.amazon.com/appconfig/latest/userguide/working-with-appconfig-extensions-about-predefined-notification-eventbridge.html)  |  This extension sends events to the EventBridge default event bus when a configuration is deployed\.   | 
|  [AWS AppConfig deployment events to Amazon Simple Notification Service \(Amazon SNS\)](https://docs.aws.amazon.com/appconfig/latest/userguide/working-with-appconfig-extensions-about-predefined-notification-sns.html)  |  This extension sends messages to an Amazon SNS topic that you specify when a configuration is deployed\.   | 
|  [AWS AppConfig deployment events to Amazon Simple Queue Service \(Amazon SQS\)](https://docs.aws.amazon.com/appconfig/latest/userguide/working-with-appconfig-extensions-about-predefined-notification-sqs.html)  |  This extension enqueues messages into your Amazon SQS queue when a configuration is deployed\.  | 
|  [Integration extension—Atlassian Jira](https://docs.aws.amazon.com/appconfig/latest/userguide/working-with-appconfig-extensions-about-jira.html)  |  This extensions allows AWS AppConfig to create and update issues whenever you make changes to a [feature flag](https://docs.aws.amazon.com/appconfig/latest/userguide/appconfig-creating-configuration-and-profile.html#appconfig-creating-configuration-and-profile-feature-flags)\.   | 