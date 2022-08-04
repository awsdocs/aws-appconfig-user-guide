# Customizing AWS authored notification extensions<a name="working-with-appconfig-extensions-creating-custom-notification"></a>

You don't have to create a Lambda or an extension to use [AWS authored notification extensions](https://docs.aws.amazon.com/appconfig/latest/userguide/working-with-appconfig-extensions-about-predefined.html)\. You can simply create an extension association and then perform an operation that calls one of the supported action points\. By default, the AWS authored notification extensions support the following actions points: 
+ `ON_DEPLOYMENT_START`
+ `ON_DEPLOYMENT_COMPLETE`
+ `ON_DEPLOYMENT_ROLLED_BACK`

If you create custom versions of the `AWS AppConfig deployment events to Amazon SNS` extension and `AWS AppConfig deployment events to Amazon SQS` extensions, you can specify the action points for which you want to receive notifications\. 

**Note**  
The `AWS AppConfig deployment events to EventBridge` extension doesn't support the `PRE_*` action points\. You can create a custom version if you want to remove some of the default actions points assigned to the AWS authored version\.

You don't need to create a Lambda function if you create custom versions of the AWS authored notification extensions\. You only need to specify an Amazon Resource Name \(ARN\) in the `Uri` field for the new extension version\.
+ For a custom EventBridge notification extension, enter the ARN of the EventBridge default events in the `Uri` field\.
+ For a custom Amazon SNS notification extension, enter the ARN of an Amazon SNS topic in the `Uri` field\.
+ For a custom Amazon SQS notification extension, enter the ARN of an Amazon SQS message queue in the `Uri` field\.