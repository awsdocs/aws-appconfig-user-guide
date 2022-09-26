# About the AWS AppConfig data plane service<a name="about-data-plane"></a>

On Nov 18, 2021, AWS AppConfig released a new data plane service\. This service replaces the previous process of retrieving configuration data by using the `GetConfiguration` API action\. The data plane service uses two new API actions, [StartConfigurationSession](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_appconfigdata_StartConfigurationSession.html) and [GetLatestConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_appconfigdata_GetLatestConfiguration.html)\. The data plane service also uses [new endpoints](https://docs.aws.amazon.com/general/latest/gr/appconfig.html#appconfigdata_data_plane)\.

If you started using AWS AppConfig before January 28, 2022, the service might be calling the `GetConfiguration` API action directly or it might be using a client provided by AWS, such as the AWS AppConfig Lambda extension, to call this API action\. If you call the `GetConfiguration` API action directly, take steps to use the `StartConfigurationSession` and `GetLatestConfiguration` API actions\. If you are using the AWS AppConfig Lambda extension, see the section titled *How this change impacts the AWS AppConfig Lambda extension* later in this topic\.

The new data plane API actions offer the following benefits over the `GetConfiguration` API action, which is now deprecated\.

1. You don't need to manage a `ClientID` parameter\. With the data plane service, `ClientID` is managed internally by the session token created by `StartConfigurationSession`\.

1. You no longer need to include `ClientConfigurationVersion` to indicate the cached version of your configuration data\. With the data plane service, `ClientConfigurationVersion` is managed internally by the session token created by `StartConfigurationSession`\.

1. The new dedicated endpoint for data plane API calls improves code structure by separating control plane and data plane calls\.

1. The new data plane service improves future extensibility for data plane operations\. By utilizing a configuration session that manages configuration data retrieval, the AWS AppConfig team can create more powerful enhancements in the future\.

**Migrating from `GetConfiguration` to `GetLatestConfiguration`**  
To start using the new data plane service, you need to update your code that calls the `GetConfiguration` API action\. Start a configuration session by using the `StartConfigurationSession` API action, and then call the `GetLatestConfiguration` API action to retrieve configuration data\. To improve performance, we recommend you cache your configuration data locally\. For more information, see [Step 6: Retrieving the configuration](appconfig-retrieving-the-configuration.md)\.

**How this change impacts the AWS AppConfig Lambda extension**  
This change has no direct impact on how the AWS AppConfig Lambda extension works\. Older versions of the AWS AppConfig Lambda extension called the `GetConfiguration` API action on your behalf\. Newer versions call the data plane API actions\. If you are using the AWS AppConfig Lambda extension, we recommend you update your extension to the most recent Amazon Resource Name \(ARN\) and update permissions for the new API calls\. For more information, see [AWS AppConfig integration with Lambda extensions](appconfig-integration-lambda-extensions.md)\.