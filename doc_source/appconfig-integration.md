# Services that integrate with AWS AppConfig<a name="appconfig-integration"></a>

AWS AppConfig integrates with other AWS services to help you manage your application configurations quickly and securely\. The following information can help you configure AWS AppConfig to integrate with the products and services you use\. 

**Topics**
+ [AWS AppConfig integration with CodePipeline](appconfig-integration-codepipeline.md)
+ [How integration works](#appconfig-integration-codepipeline-how)
+ [AWS AppConfig integration with Lambda extensions](appconfig-integration-lambda-extensions.md)

## How integration works<a name="appconfig-integration-codepipeline-how"></a>

You start by setting up and configuring CodePipeline\. This includes adding your configuration to a CodePipeline\-supported code store\. Next, you set up your AWS AppConfig environment by performing the following tasks:
+ [Create an application](https://docs.aws.amazon.com/systems-manager/latest/userguide/appconfig-creating-application.html)
+ [Create an environment](https://docs.aws.amazon.com/systems-manager/latest/userguide/appconfig-creating-environment.html)
+ [Create an AWS CodePipeline configuration profile](https://docs.aws.amazon.com/systems-manager/latest/userguide/appconfig-creating-configuration-and-profile.html)
+ [Choose a predefined deployment strategy or create your own](https://docs.aws.amazon.com/systems-manager/latest/userguide/appconfig-creating-deployment-strategy.html)

After you complete these tasks, you create a pipeline in CodePipeline that specifies AWS AppConfig as the *deploy provider*\. You can then make a change to your configuration and upload it to your CodePipeline code store\. Uploading the new configuration automatically starts a new deployment in CodePipeline\. After the deployment completes, you can verify your changes\. For information about creating a pipeline that specifies AWS AppConfig as the deploy provider, see [Tutorial: Create a Pipeline That Uses AWS AppConfig as a Deployment Provider](https://docs.aws.amazon.com/codepipeline/latest/userguide/tutorials-AppConfig.html) in the *AWS CodePipeline User Guide*\. 