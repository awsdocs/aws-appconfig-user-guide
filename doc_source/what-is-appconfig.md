# What Is AWS AppConfig?<a name="what-is-appconfig"></a>

Use [AWS AppConfig](http://aws.amazon.com/systems-manager/features/appconfig/), a capability of AWS Systems Manager, to create, manage, and quickly deploy application configurations\. You can use AWS AppConfig with applications hosted on Amazon Elastic Compute Cloud \(Amazon EC2\) instances, AWS Lambda, containers, mobile applications, or IoT devices\.

## Simplify tasks with AWS AppConfig<a name="appconfig-benefits-usage"></a>

AWS AppConfig helps simplify the following tasks:
+ **Configure**

  Source your configurations from Amazon Simple Storage Service \(Amazon S3\), AWS AppConfig hosted configurations, Parameter Store, Systems Manager Document Store\. Use AWS CodePipeline integration to source your configurations from Bitbucket Pipelines, GitHub, and AWS CodeCommit\.
+ **Validate**

  While deploying application configurations, a simple typo could cause an unexpected outage\. Prevent errors in production systems using AWS AppConfig validators\. AWS AppConfig validators provide a syntactic check using a JSON schema or a semantic check using an AWS Lambda function to ensure that your configurations deploy as intended\. Configuration deployments only proceed when the configuration data is valid\.
+ **Deploy and monitor**

  Define deployment criteria and rate controls to determine how your targets receive the new configuration\. Use AWS AppConfig deployment strategies to set deployment velocity, deployment time, and bake time\. Monitor each deployment to proactively catch any errors using AWS AppConfig integration with Amazon CloudWatch Events\. If AWS AppConfig encounters an error, the system rolls back the deployment to minimize impact on your application users\. 

## AWS AppConfig use cases<a name="appconfig-when-to-use"></a>

AWS AppConfig can help you in the following use cases:
+ **Application tuning** – Introduce changes carefully to your application that can be tested with production traffic\.
+ **Feature toggle** – Turn on new features that require a timely deployment, such as a product launch or announcement\. 
+ **Allow list** – Allow premium subscribers to access paid content\. 
+ **Operational issues** – Reduce stress on your application when a dependency or other external factor impacts the system\.

## Benefits of using AWS AppConfig<a name="learn-more-appconfig-benefits"></a>

AWS AppConfig offers the following benefits for your organization:
+ **Reduce errors in configuration changes**

  AWS AppConfig reduces application downtime by enabling you to create rules to validate your configuration\. Configurations that aren't valid can't be deployed\. AWS AppConfig provides the following two options for validating configurations:
  + For syntactic validation, you can use a JSON schema\. AWS AppConfig validates your configuration by using the JSON schema to ensure that configuration changes adhere to the application requirements\. 
  + For semantic validation, you can call an AWS Lambda function that runs your configuration before you deploy it\.
+ **Deploy changes across a set of targets quickly**

  AWS AppConfig simplifies the administration of applications at scale by deploying configuration changes from a central location\. AWS AppConfig supports configurations stored in Systems Manager Parameter Store, Systems Manager \(SSM\) documents, and Amazon S3\. You can use AWS AppConfig with applications hosted on EC2 instances, AWS Lambda, containers, mobile applications, or IoT devices\.

  Targets don't need to be configured with the Systems Manager SSM Agent or the AWS Identity and Access Management \(IAM\) instance profile required by other Systems Manager capabilities\. This means that AWS AppConfig works with unmanaged instances\.
+ **Update applications without interruptions**

  AWS AppConfig deploys configuration changes to your targets at runtime without a heavy\-weight build process or taking your targets out of service\. 
+ **Control deployment of changes across your application**

  When deploying configuration changes to your targets, AWS AppConfig enables you to minimize risk by using a deployment strategy\. You can use the rate controls of a deployment strategy to determine how fast you want your application targets to receive a configuration change\.

## **Get started with AWS AppConfig**<a name="how-to-get-started-appconfig"></a>

The following resources can help you work directly with AWS AppConfig\.

View more AWS videos on the [Amazon Web Services YouTube Channel](https://www.youtube.com/user/AmazonWebServices)\.

The following blogs can help you learn more about AWS AppConfig and its capabilities:
+ **[Introduction to AWS AppConfig deployment](http://aws.amazon.com/blogs/aws/safe-deployment-of-application-configuration-settings-with-aws-appconfig/)** – Learn about safe deployment of application configuration settings with AWS AppConfig\.
+ **[Automating feature release using AWS AppConfig deployment](http://aws.amazon.com/blogs/mt/automating-feature-release-using-aws-appconfig-integration-with-aws-codepipeline/)** – Learn how to automate feature release using AWS AppConfig integration with AWS CodePipeline\.
+ **[Deploying application configuration to serverless workloads](http://aws.amazon.com/blogs/aws/safe-deployment-of-application-configuration-settings-with-aws-appconfig/)** – Learn how to use an AWS AppConfig Lambda extension to deploy application configuration to serverless workloads\. 

## How AWS AppConfig works<a name="learn-more-appconfig-how-it-works"></a>

 At a high level, there are three processes for working with AWS AppConfig:

1. [Configure](#learn-more-appconfig-how-it-works-details-1) AWS AppConfig to work with your application\.

1. [Enable](#learn-more-appconfig-how-it-works-details-2) your application code to periodically check for and receive configuration data from AWS AppConfig\.

1. [Deploy](#learn-more-appconfig-how-it-works-details-3) a new or updated configuration\.

The following sections describe each step\.

### Configure AWS AppConfig to work with your application<a name="learn-more-appconfig-how-it-works-details-1"></a>

To configure AWS AppConfig to work with your application, you set up three types of resources, which are described in the following table\.


****  

| Resource | Details | 
| --- | --- | 
|  Application  |  An application in AWS AppConfig is a logical unit of code that provides capabilities for your customers\. For example, an application can be a microservice that runs on EC2 instances, a mobile application installed by your users, a serverless application using Amazon API Gateway and AWS Lambda, or any system you run on behalf of others\.  | 
|  Environment  |  For each application, you define one or more environments\. An environment is a logical deployment group of AWS AppConfig applications, such as applications in a `Beta` or `Production` environment\. You can also define environments for application subcomponents such as the `Web`, `Mobile`, and `Back-end` components for your application\. You can configure Amazon CloudWatch alarms for each environment\. The system monitors alarms during a configuration deployment\. If an alarm is triggered, the system rolls back the configuration\.  | 
|  Configuration profile  |  A *configuration profile* enables AWS AppConfig to access your configuration in its stored location\. You can store configurations in the following formats and locations: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/appconfig/latest/userguide/what-is-appconfig.html) A configuration profile can also include optional validators to ensure your configuration data is syntactically and semantically correct\. AWS AppConfig performs a check using the validators when you start a deployment\. If any errors are detected, the deployment stops before making any changes to the targets of the configuration\.  | 

### Enable your application code to check for and receive configuration data<a name="learn-more-appconfig-how-it-works-details-2"></a>

You must configure your application to periodically check for and receive configuration updates by using the [GetConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetConfiguration.html) API action\. When a new or updated configuration is ready, AWS AppConfig deploys the configuration file to each target in your deployment strategy\.

When called, your code sends the following information: 
+ The IDs of an AWS AppConfig application, environment, and configuration profile\.
+ A unique application instance identifier called a client ID\.
+ The last configuration version known by your application code\.

When a new configuration is deployed \(which means there is a new version of the configuration\), AWS AppConfig responds to the [GetConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetConfiguration.html) request and returns the new configuration data\.

### Deploy a new or updated configuration<a name="learn-more-appconfig-how-it-works-details-3"></a>

AWS AppConfig enables you to deploy configurations in the manner that best suits the use case of your applications\. You can deploy changes in seconds, or you can roll them out slowly to assess the impact of the changes\. The AWS AppConfig resource that helps you control deployments is called a *deployment strategy*\. A deployment strategy includes the following information:
+ Total amount of time for a deployment to last\. \([DeploymentDurationInMinutes](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_DeploymentStrategy.html)\)\.
+ The percentage of targets to receive a deployed configuration during each interval\. \([GrowthFactor](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_DeploymentStrategy.html)\)\.
+ The amount of time AWS AppConfig monitors for alarms before considering the deployment to be complete and no longer eligible for automatic rollback\. \([FinalBakeTimeInMinutes](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_DeploymentStrategy.html)\)\.

You can use built\-in deployment strategies that cover common scenarios, or you can create your own\. After you create or choose a deployment strategy, you start the deployment\. Starting the deployment calls the [StartDeployment](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_StartDeployment.html) API action\. The call includes the IDs of an application, environment, configuration profile, and \(optionally\) the configuration data version to deploy\. The call also includes the ID of the deployment strategy to use, which determines how the configuration data rolls out\.

**Note**  
For information about AWS AppConfig language\-specific SDKs, see [AWS AppConfig SDKs](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_CreateApplication.html#API_CreateApplication_SeeAlso)\.

## Pricing for AWS AppConfig<a name="learn-more-appconfig-cost"></a>

There is a charge to use AWS AppConfig\. For more information, see [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\.

## AWS AppConfig quotas<a name="learn-more-service-quotas"></a>

Information about AWS AppConfig endpoints and service quotas along with other Systems Manager quotas is in the [Amazon Web Services General Reference](https://docs.aws.amazon.com/general/latest/gr/ssm.html)\.

**Note**  
For information about quotas for services that store AWS AppConfig configurations, see [About configuration store quotas and limitations](appconfig-creating-configuration-and-profile.md#appconfig-creating-configuration-and-profile-quotas)\.