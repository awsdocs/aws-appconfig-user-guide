# Step 6: Retrieving the configuration<a name="appconfig-retrieving-the-configuration"></a>

Your application retrieves configuration data by first establishing a configuration session using the [StartConfigurationSession](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_appconfigdata_StartConfigurationSession.html) API action\. Your session's client then makes periodic calls to [GetLatestConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_appconfigdata_GetLatestConfiguration.html) to check for and retrieve the latest data available\.

When calling `StartConfigurationSession`, your code sends the following information:
+ Identifiers \(ID or name\) of an AWS AppConfig application, environment, and configuration profile that the session tracks\.
+ \(Optional\) The minimum amount of time the session's client must wait between calls to `GetLatestConfiguration`\.

In response, AWS AppConfig provides an `InitialConfigurationToken` to be given to the session's client and used the first time it calls `GetLatestConfiguration` for that session\.

**Important**  
This token should only be used once in your first call to `GetLatestConfiguration`\. You *must* use the new token in the `GetLatestConfiguration` response \(`NextPollConfigurationToken`\) in each subsequent call to `GetLatestConfiguration`\.

When calling `GetLatestConfiguration`, your client code sends the most recent `ConfigurationToken` value it has and receives in response:
+ `NextPollConfigurationToken`: the `ConfigurationToken` value to use on the next call to `GetLatestConfiguration`\.
+ `NextPollIntervalInSeconds`: the duration the client should wait before making its next call to `GetLatestConfiguration`\. This duration may vary over the course of the session, so it should be used instead of the value sent on the `StartConfigurationSession` call\.
+ The configuration: the latest data intended for the session\. This may be empty if the client already has the latest version of the configuration\.

**Important**  
Note the following important information\.  
The [StartConfigurationSession](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_appconfigdata_StartConfigurationSession.html) API should only be called once per application, environment, configuration profile, and client to establish a session with the service\. This is typically done in the startup of your application or immediately prior to the first retrieval of a configuration\.
The `InitialConfigurationToken` and `NextPollConfigurationToken` expire after 24 hours\. If a `GetLatestConfiguration` call uses an expired token, the system returns `BadRequestException`\.
The API action previously used to retrieve configuration data, `GetConfiguration`, is deprecated\. 

## Retrieving a configuration example<a name="appconfig-retrieving-the-configuration-example"></a>

The following AWS CLI example demonstrates how to retrieve configuration data by using the AWS AppConfig Data `StartConfigurationSession` and `GetLatestConfiguration` API actions\. The first command starts a configuration session\. This call includes the IDs \(or names\) of the AWS AppConfig application, the environment, and the configuration profile\. The API returns an `InitialConfigurationToken` used to fetch your configuration data\.

```
aws appconfigdata start-configuration-session \
    --application-identifier application_name_or_ID \
    --environment-identifier environment_name_or_ID \
    --configuration-profile-identifier configuration_profile_name_or_ID
```

The system responds with information in the following format\.

```
{
   "InitialConfigurationToken": initial configuration token
}
```

After starting a session, use [InitialConfigurationToken](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_appconfigdata_StartConfigurationSession.html#API_appconfigdata_StartConfigurationSession_ResponseSyntax) to call [GetLatestConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_appconfigdata_GetLatestConfiguration.html) to fetch your configuration data\. The configuration data is saved to the `mydata.json` file\.

```
aws appconfigdata get-latest-configuration \
    --configuration-token initial configuration token mydata.json
```

The first call to `GetLatestConfiguration` uses the `ConfigurationToken` obtained from `StartConfigurationSession`\. The following information is returned\.

```
{
    "NextPollConfigurationToken" : next configuration token,
    "ContentType" : content type of configuration,
    "NextPollIntervalInSeconds" : 60
}
```

Subsequent calls to `GetLatestConfiguration` *must* provide `NextPollConfigurationToken` from the previous response\.

```
aws appconfigdata get-latest-configuration \
    --configuration-token next configuration token mydata.json
```

**Important**  
Note the following important details about the `GetLatestConfiguration` API action:  
The `GetLatestConfiguration` response includes a `Configuration` section that shows the configuration data\. The `Configuration` section only appears if the system finds new or updated configuration data\. If the system doesn't find new or updated configuration data, then the `Configuration` data is empty\. 
You receive a new `ConfigurationToken` in every response from `GetLatestConfiguration`\.
We recommend tuning the polling frequency of your `GetLatestConfiguration` API calls based on your budget, the expected frequency of your configuration deployments, and the number of targets for a configuration\.