# Step 6: Receiving the configuration<a name="appconfig-retrieving-the-configuration"></a>

To retrieve a deployed configuration, you must start a Configuration Session\. The following AWS CLI command demonstrates how to start a Configuration Session\. This call includes the IDs \(or names\) of the AWS AppConfig application, the environment, and the configuration profile\. The API returns an `InitialConfigurationToken` used to fetch your configuration data\.

**Note**  
The [StartConfigurationSession](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_StartConfigurationSession.html) API should only be called once per application, environment, configuration profile, and client to establish a session with the service\. This is typically done in the startup of your application or immediately prior to the first retrieval of a configuration\.

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

 After starting a session, use [InitialConfigurationToken](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_StartConfigurationSession.html#API_StartConfigurationSession_ResponseSyntax) to call [GetLatestConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetLatestConfiguration.html) to fetch your configuration data\. The configuration data is saved to the `mydata.json` file\.

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

Subsequent calls to `GetLatestConfiguration` MUST provide `NextPollConfigurationToken` from the previous response\.

```
aws appconfigdata get-latest-configuration \
    --configuration-token next configuration token mydata.json
```

**Important**  
Note the following important details about the `GetLatestConfiguration` API action:  
The `GetLatestConfiguration` response includes a `Configuration` section that shows the configuration data\. The `Configuration` section only appears if the system finds new or updated configuration data\. If the system doesn't find new or updated configuration data, then the `Configuration` data is empty\. 
You receive a new `ConfigurationToken` in every response from `GetLatestConfiguration`\.
We recommend tuning the polling frequncy of your `GetLatestConfiguration` API calls based on your budget, the expected frequency of your configuration deployments, and the number of targets for a configuration\.