# AWS AppConfig integration with Lambda extensions<a name="appconfig-integration-lambda-extensions"></a>

An AWS Lambda extension is a companion process that augments the capabilities of a Lambda function\. An extension can start before a function is invoked, run in parallel with a function, and continue to run after a function invocation is processed\. In essence, a Lambda extension is like a client that runs in parallel to a Lambda invocation\. This parallel client can interface with your function at any point during its lifecycle\.

If you use AWS AppConfig to manage configurations for a Lambda function, then we recommend that you add the AWS AppConfig Lambda extension\. This extension includes best practices that simplify using AWS AppConfig while reducing costs\. Reduced costs result from fewer API calls to the AWS AppConfig service and, separately, reduced costs from shorter Lambda function processing times\.

This topic includes information about the AWS AppConfig Lambda extension and the procedure for how to configure the extension to work with your Lambda function\. For more information about Lambda extensions, see [Lambda extensions](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-extensions-api.html) in the *AWS Lambda Developer Guide*\.

**Note**  
The AWS AppConfig Lambda extension currently doesn't support the ARM architecture\.

## How it works<a name="appconfig-integration-lambda-extensions-how-it-works"></a>

If you use AWS AppConfig to manage configurations for a Lambda function *without* Lambda extensions, then you must configure your Lambda function to receive configuration updates by integrating with the [https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_StartConfigurationSession.html](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_StartConfigurationSession.html) and [https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetLatestConfiguration.html](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetLatestConfiguration.html) API actions\.

  To get your configuration data, you first call `StartConfigurationSession` with the names or IDs of the application, environment, and configuration profile for your data, then call `GetLatestConfiguration` using the `StartConfigurationSession` result’s `InitialTokenConfiguration`\. To make sure your Lambda function stays up\-to\-date, you need a mechanism to periodically check for configuration changes over a certain frequency, and manage a cache to provide the configuration data between the checks AWS AppConfig\. Regardless of whether your configuration data has been updated, each invocation to `GetLatestConfiguration` returns a new *`NextPollConfigurationToken`*, which is the value to be passed in the next `GetLatestConfiguration` call\.  

Integrating the AWS AppConfig Lambda extension with your Lambda function simplifies this process\. The extension takes care of calling the AWS AppConfig service, managing a local cache of retrieved data, tracking the configuration tokens needed for the next service calls, and periodically checking for configuration updates in the background\. The following diagram shows how it works\.

![\[A diagram of how the AWS AppConfig Lambda extension works\]](http://docs.aws.amazon.com/appconfig/latest/userguide/images/AppConfigLambdaExtension.png)

1. You configure the AWS AppConfig Lambda extension as a layer of your Lambda function\. 

1. To access its configuration data, your function calls the AWS AppConfig extension at an HTTP endpoint running on `localhost:2772`\.

1. The extension maintains a local cache of the configuration data\. If the data isn't in the cache, the extension calls AWS AppConfig to get the configuration data\.

1. Upon receiving the configuration from the service, the extension stores it in the local cache and passes it to the Lambda function\. 

1. AWS AppConfig Lambda extension periodically checks for updates to your configuration data in the background\. Each time your Lambda function is invoked, the extension checks the elapsed time since it retrieved a configuration\. If the elapsed time is greater than the configured poll interval, the extension calls AWS AppConfig to check for newly deployed data, updates the local cache if there has been a change, and resets the elapsed time\. 

**Note**  
Lambda instantiates separate instances corresponding to the concurrency level that your function requires\. Each instance is isolated and maintains its own local cache of your configuration data\. For more information about Lambda instances and concurrency, see [Managing concurrency for a Lambda function](https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html)\.
The amount of time it takes for a configuration change to appear in a Lambda function, after you deploy an updated configuration from AWS AppConfig, depends on the deployment strategy you used for the deployment and the polling interval you configured for the extension\. 

## Before you begin<a name="appconfig-integration-lambda-extensions-before-you-begin"></a>

Before you enable the AWS AppConfig Lambda extension, do the following:
+ Organize the configurations in your Lambda function so that you can externalize them into AWS AppConfig\.
+ Configure AWS AppConfig to manage your configuration updates\. For more information, see [Working with AWS AppConfig](appconfig-working.md)\.
+ Add `appconfig:StartConfigurationSession` and `appconfig:GetLatestConfiguration` to the AWS Identity and Access Management \(IAM\) policy used by the Lambda function execution role\. For more information, see [AWS Lambda execution role](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html) in the *AWS Lambda Developer Guide*\. For more information about AWS AppConfig permissions, see [Actions, resources, and condition keys for AWS AppConfig](https://docs.aws.amazon.com/service-authorization/latest/reference/list_awsappconfig.html) in the *Service Authorization Reference*\. 

## Adding the AWS AppConfig Lambda extension<a name="appconfig-integration-lambda-extensions-add"></a>

To use the AWS AppConfig Lambda extension, you need to add the extension to your Lambda\. This can be done by adding the AWS AppConfig Lambda extension to your Lambda function as a layer or by enabling the extension on a Lambda function as a container image\.

### Using layer and ARN to add the AWS AppConfig Lambda extension<a name="appconfig-integration-lambda-extensions-enabling"></a>

To use the AWS AppConfig Lambda extension, you add the extension to your Lambda function as a layer\. For information about how to add a layer to your function, see [Configuring extensions](https://docs.aws.amazon.com/lambda/latest/dg/using-extensions.html#using-extensions-config) in the *AWS Lambda Developer Guide*\. The name of the extension in the AWS Lambda console is **AWS\-AppConfig\-Extension**\. Also note that when you add the extension as a layer to your Lambda, you must specify an Amazon Resource Name \(ARN\)\. Choose an ARN from the following list that corresponds with the AWS Region where you created the Lambda\.


****  

| Region | ARN | 
| --- | --- | 
|  US East \(N\. Virginia\)  | `arn:aws:lambda:us-east-1:027255383542:layer:AWS-AppConfig-Extension:61` | 
|  US East \(Ohio\)  |  `arn:aws:lambda:us-east-2:728743619870:layer:AWS-AppConfig-Extension:47`  | 
|  US West \(N\. California\)  |  `arn:aws:lambda:us-west-1:958113053741:layer:AWS-AppConfig-Extension:61`  | 
|  US West \(Oregon\)  |  `arn:aws:lambda:us-west-2:359756378197:layer:AWS-AppConfig-Extension:89`  | 
|  Canada \(Central\)  |  `arn:aws:lambda:ca-central-1:039592058896:layer:AWS-AppConfig-Extension:47`  | 
|  Europe \(Frankfurt\)  |  `arn:aws:lambda:eu-central-1:066940009817:layer:AWS-AppConfig-Extension:54`  | 
|  Europe \(Ireland\)  |  `arn:aws:lambda:eu-west-1:434848589818:layer:AWS-AppConfig-Extension:59` | 
|  Europe \(London\)  |  `arn:aws:lambda:eu-west-2:282860088358:layer:AWS-AppConfig-Extension:47`  | 
|  Europe \(Paris\)  |  `arn:aws:lambda:eu-west-3:493207061005:layer:AWS-AppConfig-Extension:48`  | 
|  Europe \(Stockholm\)  |  `arn:aws:lambda:eu-north-1:646970417810:layer:AWS-AppConfig-Extension:86`  | 
|  Europe \(Milan\)  |  `arn:aws:lambda:eu-south-1:203683718741:layer:AWS-AppConfig-Extension:44`  | 
| China \(Bejing\) | `arn:aws-cn:lambda:cn-north-1:615057806174:layer:AWS-AppConfig-Extension:43` | 
| China \(Ningxia\) | `arn:aws-cn:lambda:cn-northwest-1:615084187847:layer:AWS-AppConfig-Extension:43` | 
|  Asia Pacific \(Hong Kong\)  |  `arn:aws:lambda:ap-east-1:630222743974:layer:AWS-AppConfig-Extension:44`  | 
|  Asia Pacific \(Tokyo\)  | `arn:aws:lambda:ap-northeast-1:980059726660:layer:AWS-AppConfig-Extension:45` | 
| Asia Pacific \(Osaka\) | `arn:aws:lambda:ap-northeast-3:706869817123:layer:AWS-AppConfig-Extension:42` | 
|  Asia Pacific \(Seoul\)  | `arn:aws:lambda:ap-northeast-2:826293736237:layer:AWS-AppConfig-Extension:54` | 
|  Asia Pacific \(Singapore\)  | `arn:aws:lambda:ap-southeast-1:421114256042:layer:AWS-AppConfig-Extension:45` | 
|  Asia Pacific \(Sydney\)  |  `arn:aws:lambda:ap-southeast-2:080788657173:layer:AWS-AppConfig-Extension:54`  | 
| Asia Pacific \(Jakarta\) | `arn:aws:lambda:ap-southeast-3:418787028745:layer:AWS-AppConfig-Extension:13` | 
|  Asia Pacific \(Mumbai\)  |  `arn:aws:lambda:ap-south-1:554480029851:layer:AWS-AppConfig-Extension:55`  | 
|  South America \(São Paulo\)  | `arn:aws:lambda:sa-east-1:000010852771:layer:AWS-AppConfig-Extension:61` | 
|  Africa \(Cape Town\)  |  `arn:aws:lambda:af-south-1:574348263942:layer:AWS-AppConfig-Extension:44`  | 
|  Middle East \(Bahrain\)  |  `arn:aws:lambda:me-south-1:559955524753:layer:AWS-AppConfig-Extension:44`  | 
| AWS GovCloud \(US\-East\) | `arn:aws-us-gov:lambda:us-gov-east-1:946561847325:layer:AWS-AppConfig-Extension:20` | 
| AWS GovCloud \(US\-West\) | `arn:aws-us-gov:lambda:us-gov-west-1:946746059096:layer:AWS-AppConfig-Extension:20` | 

If you want to test the extension before you add it to your function, you can verify that it works by using the following code example\.

```
import urllib.request
                

def lambda_handler(event, context):
    url = f'http://localhost:2772/applications/application_name/environments/environment_name/configurations/configuration_name'
    config = urllib.request.urlopen(url).read()
    return config
```

To test it, create a new Lambda function for Python, add the extension, and then run the Lambda function\. After you run the Lambda function, the AWS AppConfig Lambda function returns the configuration you specified for the http://localhost:2772 path\. For information about creating a Lambda function, see [Create a Lambda function with the console](https://docs.aws.amazon.com/lambda/latest/dg/getting-started-create-function.html) in the *AWS Lambda Developer Guide*\. 

To add the AWS AppConfig Lambda extension as a container image, see [Using a container image to add the AWS AppConfig Lambda extension](appconfig-integration-lambda-extensions-container-image.md)\.

## Configuring AWS AppConfig Lambda extension<a name="appconfig-integration-lambda-extensions-config"></a>

You can configure the extension by changing the following AWS Lambda environment variables\. For more information, see [Using AWS Lambda environment variables](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars.html) in the *AWS Lambda Developer Guide*\. 

**Prefetching configuration data**

The environment variable `AWS_APPCONFIG_EXTENSION_PREFETCH_LIST` can improve the start\-up time of your function\. When the AWS AppConfig Lambda extension is initialized, it retrieves the specified configuration from AWS AppConfig before Lambda starts to initialize your function and invoke your handler\. In some cases, the configuration data is already available in the local cache before your function requests it\. 

To use the prefetch capability, set the value of the environment variable to the path corresponding to your configuration data\. For example, if your configuration corresponds to an application, environment, and configuration profile respectively named "my\_application", "my\_environment", and "my\_configuration\_data", the path would be `/applications/my_application/environments/my_environment/configurations/my_configuration_data`\. You can specify multiple configuration items by listing them as a comma\-separated list \(If you have a resource name that includes a comma, use the resource’s ID value instead of its name\)\. 

**Accessing configuration data from another account**

The AWS AppConfig Lambda extension can retrieve configuration data from another account by specifying an IAM role that grants [permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_permissions-to-switch.html) to the data\. To set this up, follow these steps: 

1. In the account where AWS AppConfig is used to manage the configuration data, create a role with a trust policy that grants the account running the Lambda function access to the `appconfig:StartConfigurationSession` and `appconfig:GetLatestConfiguration` actions, along with the partial or complete ARNs corresponding to the AWS AppConfig configuration resources\.

1. In the account running the Lambda function, add the `AWS_APPCONFIG_EXTENSION_ROLE_ARN` environment variable to the Lambda function with the ARN of the role created in step 1\.

1. \(Optional\) If needed, an [external ID](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html) can be specified using the `AWS_APPCONFIG_EXTENSION_ROLE_EXTERNAL_ID` environment variable\. Similarly, a session name can be configured using the `AWS_APPCONFIG_EXTENSION_ROLE_SESSION_NAME` environment variable\.

**Note**  
The AWS AppConfig Lambda extension can only retrieve data from one account\. If you specify an IAM role, the extension will not be able to retrieve configuration data from the account in which the Lambda function is running\.

**Note**  
AWS Lambda logs execution information about the AWS AppConfig Lambda extension along with the Lambda function by using Amazon CloudWatch Logs\. 


****  

| Environment variable | Details | Default value | 
| --- | --- | --- | 
|  `AWS_APPCONFIG_EXTENSION_POLL_INTERVAL_SECONDS`  |  This environment variable controls how often the extension polls AWS AppConfig for an updated configuration in seconds\.  | 45 | 
|  `AWS_APPCONFIG_EXTENSION_POLL_TIMEOUT_MILLIS`  |  This environment variable controls the maximum amount of time, in milliseconds, the extension waits for a response from AWS AppConfig when refreshing data in the cache\. If AWS AppConfig does not respond in the specified amount of time, the extension skips this poll interval and returns the previously updated cached data\.  | 3000 | 
|  `AWS_APPCONFIG_EXTENSION_HTTP_PORT`  |  This environment variable specifies the port on which the local HTTP server that hosts the extension runs\.  | 2772 | 
|  `AWS_APPCONFIG_EXTENSION_PREFETCH_LIST`  |  This environment variable specifies the configuration data that the extension starts to retrieve before the function initializes and the handler runs\. It can reduce the function's cold start time significantly\.  | None | 
|  `AWS_APPCONFIG_EXTENSION_MAX_CONNECTIONS`  |  This environment variable configures the maximum number of connections the extension uses to retrieve configurations from AWS AppConfig\.   | 3 | 
|  `AWS_APPCONFIG_EXTENSION_LOG_LEVEL`  |  This environment variable specifies which AWS AppConfig extension\-specific logs are sent to Amazon CloudWatch Logs for a function\. Valid, case\-insensitive values are: `debug`, `info`, `warn`, `error`, and `none`\. Debug includes detailed information, including timing information, about the extension\.  |  `info`  | 
| AWS\_APPCONFIG\_EXTENSION\_ROLE\_ARN | This environment variable specifies the IAM role ARN corresponding to a role that should be assumed by the AWS AppConfig extension to retrieve configuration\. | None | 
| AWS\_APPCONFIG\_EXTENSION\_ROLE\_EXTERNAL\_ID | This environment variable specifies the external id to use in conjunction with the assumed role ARN\. | None | 
| AWS\_APPCONFIG\_EXTENSION\_ROLE\_SESSION\_NAME | This environment variable specifies the session name to be associated with the credentials for the assumed IAM role\. | None | 
| AWS\_APPCONFIG\_EXTENSION\_SERVICE\_REGION | This environment variable specifies an alternative region the extension should use to call the AWS AppConfig service\. When undefined, the extension uses the endpoint in the current region\. | None | 

## <a name="appconfig-integration-lambda-extensions-enabling"></a>