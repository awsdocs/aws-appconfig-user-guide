# Step 5: Deploying a configuration<a name="appconfig-deploying"></a>

Starting a deployment in AWS AppConfig calls the [StartDeployment](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_StartDeployment.html) API action\. This call includes the IDs of the AWS AppConfig application, the environment, the configuration profile, and \(optionally\) the configuration data version to deploy\. The call also includes the ID of the deployment strategy to use, which determines how the configuration data is deployed\.

AWS AppConfig monitors the distribution to all hosts and reports status\. If a distribution fails, then AWS AppConfig rolls back the configuration\.

**Note**  
You can only deploy one configuration at a time to an Environment\. However, you can deploy one configuration each to different Environments at the same time\.

## Deploy a configuration \(console\)<a name="appconfig-deploying-console"></a>

Use the following procedure to deploy an AWS AppConfig configuration by using the AWS Systems Manager console\.

**To deploy a configuration by using the console**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/appconfig/](https://console.aws.amazon.com/systems-manager/appconfig/)\.

1. In the navigation pane, choose **AWS AppConfig**\.

1. On the **Applications** tab, choose an application, and then choose **View details**\.

1. On the **Environments** tab, choose an environment, and then choose **View details**\.

1. Choose **Start deployment**\.

1. For **Configuration**, choose a configuration from the list\.

1. Depending on the source of your configuration, use the **Document version** or **Parameter version** list to choose the version you want to deploy\. 

1. For **Deployment strategy**, choose a strategy from the list\.

1. For **Deployment description**, enter a description\. 

1. In the **Tags** section, enter a key and an optional value\. You can specify a maximum of 50 tags for a resource\. 

1. Choose **Start deployment**\.

## Deploy a configuration \(commandline\)<a name="appconfig-deploying-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to deploy a AWS AppConfig configuration\.

**To deploy a configuration step by step**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to deploy a configuration\. 

------
#### [ Linux ]

   ```
   aws appconfig start-deployment \
     --application-id The_application_ID \
     --environment-id The_environment_ID \
     --deployment-strategy-id The_deployment_strategy_ID \
     --configuration-profile-id The_configuration_profile_ID \
     --configuration-version The_configuration_version_to_deploy \
     --description A_description_of_the_deployment \
     --tags User_defined_key_value_pair_metadata_of_the_deployment
   ```

------
#### [ Windows ]

   ```
   aws appconfig start-deployment ^
     --application-id The_application_ID ^
     --environment-id The_environment_ID ^
     --deployment-strategy-id The_deployment_strategy_ID ^
     --configuration-profile-id The_configuration_profile_ID ^
     --configuration-version The_configuration_version_to_deploy ^
     --description A_description_of_the_deployment ^
     --tags User_defined_key_value_pair_metadata_of_the_deployment
   ```

------
#### [ PowerShell ]

   ```
   Start-APPCDeployment `
     -ApplicationId The_application_ID `
     -ConfigurationProfileId The_configuration_profile_ID `
     -ConfigurationVersion The_configuration_version_to_deploy `
     -DeploymentStrategyId The_deployment_strategy_ID `
     -Description A_description_of_the_deployment `
     -EnvironmentId The_environment_ID `
     -Tag Hashtable_type_user_defined_key_value_pair_metadata_of_the_deployment
   ```

------

   The system returns information like the following\.

------
#### [ Linux ]

   ```
   {   
      "ApplicationId": "The ID of the application that was deployed",
      "EnvironmentId" : "The ID of the environment",
      "DeploymentStrategyId": "The ID of the deployment strategy that was deployed",
      "ConfigurationProfileId": "The ID of the configuration profile that was deployed",
      "DeploymentNumber": The sequence number of the deployment,
      "ConfigurationName": "The name of the configuration",
      "ConfigurationLocationUri": "Information about the source location of the configuration",
      "ConfigurationVersion": "The configuration version that was deployed",
      "Description": "The description of the deployment",
      "DeploymentDurationInMinutes": Total amount of time the deployment lasted,
      "GrowthType": "The linear or exponential algorithm used to define how percentage grew over time",
      "GrowthFactor": The percentage of targets to receive a deployed configuration during each interval,
      "FinalBakeTimeInMinutes": Time AWS AppConfig monitored for alarms before considering the deployment to be complete,
      "State": "The state of the deployment",  
   
      "EventLog": [ 
         { 
            "Description": "A description of the deployment event",
            "EventType": "The type of deployment event",
            "OccurredAt": The date and time the event occurred,
            "TriggeredBy": "The entity that triggered the deployment event"
         }
      ],
   
      "PercentageComplete": The percentage of targets for which the deployment is available,
      "StartedAt": The time the deployment started,
      "CompletedAt": The time the deployment completed   
   }
   ```

------
#### [ Windows ]

   ```
   {
      "ApplicationId": "The ID of the application that was deployed",
      "EnvironmentId" : "The ID of the environment",
      "DeploymentStrategyId": "The ID of the deployment strategy that was deployed",
      "ConfigurationProfileId": "The ID of the configuration profile that was deployed",
      "DeploymentNumber": The sequence number of the deployment,
      "ConfigurationName": "The name of the configuration",
      "ConfigurationLocationUri": "Information about the source location of the configuration",
      "ConfigurationVersion": "The configuration version that was deployed",
      "Description": "The description of the deployment",
      "DeploymentDurationInMinutes": Total amount of time the deployment lasted,
      "GrowthType": "The linear or exponential algorithm used to define how percentage grew over time",
      "GrowthFactor": The percentage of targets to receive a deployed configuration during each interval,
      "FinalBakeTimeInMinutes": Time AWS AppConfig monitored for alarms before considering the deployment to be complete,
      "State": "The state of the deployment",  
   
      "EventLog": [ 
         { 
            "Description": "A description of the deployment event",
            "EventType": "The type of deployment event",
            "OccurredAt": The date and time the event occurred,
            "TriggeredBy": "The entity that triggered the deployment event"
         }
      ],
   
      "PercentageComplete": The percentage of targets for which the deployment is available,
      "StartedAt": The time the deployment started,
      "CompletedAt": The time the deployment completed 
   }
   ```

------
#### [ PowerShell ]

   ```
   ApplicationId               : The ID of the application that was deployed
   CompletedAt                 : The time the deployment completed
   ConfigurationLocationUri    : Information about the source location of the configuration
   ConfigurationName           : The name of the configuration
   ConfigurationProfileId      : The ID of the configuration profile that was deployed
   ConfigurationVersion        : The configuration version that was deployed
   ContentLength               : Runtime of the deployment 
   DeploymentDurationInMinutes : Total amount of time the deployment lasted
   DeploymentNumber            : The sequence number of the deployment
   DeploymentStrategyId        : The ID of the deployment strategy that was deployed
   Description                 : The description of the deployment
   EnvironmentId               : The ID of the environment that was deployed
   EventLog                    : {Description : A description of the deployment event, EventType : The type of deployment event, OccurredAt : The date and time the event occurred,
            TriggeredBy : The entity that triggered the deployment event}
   FinalBakeTimeInMinutes      : Time AWS AppConfig monitored for alarms before considering the deployment to be complete
   GrowthFactor                : The percentage of targets to receive a deployed configuration during each interval
   GrowthType                  : The linear or exponential algorithm used to define how percentage grew over time
   HttpStatusCode              : HTTP Status of the runtime
   PercentageComplete          : The percentage of targets for which the deployment is available
   ResponseMetadata            : Runtime Metadata
   StartedAt                   : The time the deployment started
   State                       : The state of the deployment
   ```

------