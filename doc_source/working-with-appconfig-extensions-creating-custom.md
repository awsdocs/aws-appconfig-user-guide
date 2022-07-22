# Creating custom AWS AppConfig extensions<a name="working-with-appconfig-extensions-creating-custom"></a>

To create a custom AWS AppConfig extension, complete the following tasks\. Each task is described in more detail in later topics\.

**1\. Create an AWS Lambda function**  
For most use\-cases, to create a custom extension, you must create an AWS Lambda function to perform any computation and processing defined in the extension\. An exception to this rule is if you create *custom* versions of the [AWS\-authored notification extensions](https://docs.aws.amazon.com/appconfig/latest/userguide/working-with-appconfig-extensions-about-predefined.html) to add or remove action points\. This exception is described in more detail in [Creating a custom AWS AppConfig extension](working-with-appconfig-extensions-creating-custom-extensions.md)\.

**2\. Configure permissions for your custom extension**  
To configure permissions for your custom extension, you can do one of the following:  
+ Create an AWS Identity and Access Management \(IAM\) service role that includes `InvokeFunction` permissions\. 
+ Create a resource policy by using the Lambda [AddPermission](https://docs.aws.amazon.com/lambda/latest/dg/API_AddPermission.html) API action\.
This walkthrough describes how to create the IAM service role\.

**3\. Create an extension**  
You can create an extension by using the AWS AppConfig console\.

**4\. Create an extension association**  
You can create an extension association by using the AWS AppConfig console\.

**5\. Perform an action that invokes the extension**  
After you create the association, AWS AppConfig invokes the extension when the action points defined by the extension occur for that resource\. For example, if you associate an extension that contains a `PRE_CREATE_HOSTED_CONFIGURATION_VERSION` action, the extension will be invoked every time you create a new hosted configuration version\.

The topics in this section describe how to create a custom AWS AppConfig extension\. Each task involved in creating an extension is described in the context of a use case where a customer wants to create an extension that automatically backs up a configuration to an Amazon Simple Storage Service \(Amazon S3\) bucket\. The extension runs whenever a hosted configuration is created \(`PRE_CREATE_HOSTED_CONFIGURATION_VERSION`\) or deployed \(`PRE_START_DEPLOYMENT`\)\.

**Topics**
+ [Creating a Lambda function for a custom AWS AppConfig extension](working-with-appconfig-extensions-creating-custom-lambda.md)
+ [Configuring permissions for a custom AWS AppConfig extension](working-with-appconfig-extensions-creating-custom-permissions.md)
+ [Creating a custom AWS AppConfig extension](working-with-appconfig-extensions-creating-custom-extensions.md)
+ [Creating an extension association for a custom AWS AppConfig extension](working-with-appconfig-extensions-creating-custom-association.md)
+ [Performing an action that invokes a custom AWS AppConfig extension](working-with-appconfig-extensions-creating-custom-invoke.md)