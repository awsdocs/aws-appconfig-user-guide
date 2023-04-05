# Step 4: Creating a deployment strategy<a name="appconfig-creating-deployment-strategy"></a>

An AWS AppConfig deployment strategy defines the following important aspects of a configuration deployment\.


****  

| Setting | Description | 
| --- | --- | 
|  Deployment type  | Deployment type defines how the configuration deploys or *rolls out*\. AWS AppConfig supports **Linear** and **Exponential** deployment types\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/appconfig/latest/userguide/appconfig-creating-deployment-strategy.html)  | 
|  Step percentage \(growth factor\)  |  This setting specifies the percentage of callers to target during each step of the deployment\.  In the SDK and the [AWS AppConfig API Reference](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_CreateDeploymentStrategy.html), `step percentage` is called `growth factor`\.   | 
|  Deployment time  |  This setting specifies an amount of time during which AWS AppConfig deploys to hosts\. This is not a timeout value\. It is a window of time during which the deployment is processed in intervals\. | 
|  Bake time  |  This setting specifies the amount of time AWS AppConfig monitors for Amazon CloudWatch alarms after the configuration has been deployed to 100% of its targets, before considering the deployment to be complete\. If an alarm is triggered during this time, AWS AppConfig rolls back the deployment\. You must configure permissions for AWS AppConfig to roll back based on CloudWatch alarms\. For more information, see [\(Optional\) Configure permissions for rollback based on CloudWatch alarms](setting-up-appconfig.md#getting-started-with-appconfig-cloudwatch-alarms-permissions)\.  | 

## Predefined deployment strategies<a name="appconfig-creating-deployment-strategy-predefined"></a>

AWS AppConfig includes predefined deployment strategies to help you quickly deploy a configuration\. Instead of creating your own strategies, you can choose one of the following when you deploy a configuration\.


****  

| Deployment strategy | Description | 
| --- | --- | 
|  AppConfig\.AllAtOnce  | **Quick**: This strategy deploys the configuration to all targets immediately\. The system monitors for Amazon CloudWatch alarms for 10 minutes\. If no alarms are received in this time, the deployment is complete\. If an alarm is triggered during this time, AppConfig rolls back the deployment\.   | 
|  AppConfig\.Linear50PercentEvery30Seconds  | **Testing/Demonstration**: This strategy deploys the configuration to half of all targets every 30 seconds for a one\-minute deployment\. The system monitors for Amazon CloudWatch alarms for 1 minute\. If no alarms are received in this time, the deployment is complete\. If an alarm is triggered during this time, AppConfig rolls back the deployment\.We recommend using this strategy only for testing or demonstration purposes because it has a short duration and bake time\.  | 
|  AppConfig\.Canary10Percent20Minutes  | **AWS Recommended**: This strategy processes the deployment exponentially using a 10% growth factor over 20 minutes\. The system monitors for Amazon CloudWatch alarms for 10 minutes\. If no alarms are received in this time, the deployment is complete\. If an alarm is triggered during this time, AppConfig rolls back the deployment\.We recommend using this strategy for production deployments because it aligns with AWS best practices for configuration deployments\.  | 

## Create a deployment strategy<a name="appconfig-creating-deployment-strategy-create"></a>

You can create a maximum of 20 deployment strategies\. When you deploy a configuration, you can choose the deployment strategy that works best for the application and the environment\.

### Creating an AWS AppConfig deployment strategy \(console\)<a name="appconfig-creating-deployment-strategy-create-console"></a>

Use the following procedure to create an AWS AppConfig deployment strategy by using the AWS Systems Manager console\.

**To create a deployment strategy**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/appconfig/](https://console.aws.amazon.com/systems-manager/appconfig/)\.

1. In the navigation pane, choose **AWS AppConfig**\.

1. Choose the **Deployment Strategies** tab, and then choose **Create deployment strategy**\.

1. For **Name**, enter a name for the deployment strategy\.

1. For **Description**, enter information about the deployment strategy\.

1. For **Deployment type**, choose a type\.

1. For **Step percentage**, choose the percentage of callers to target during each step of the deployment\. 

1. For **Deployment time**, enter the total duration for the deployment in minutes or hours\. 

1. For **Bake time**, enter the total time, in minutes or hours, to monitor for Amazon CloudWatch alarms before proceeding to the next step of a deployment or before considering the deployment to be complete\. 

1. In the **Tags** section, enter a key and an optional value\. You can specify a maximum of 50 tags for a resource\. 

1. Choose **Create deployment strategy**\.

**Important**  
If you created a configuration profile for AWS CodePipeline, then you must create a pipeline in CodePipeline that specifies AWS AppConfig as the *deploy provider*\. You don't need to perform [Step 5: Deploying a configuration](appconfig-deploying.md)\. However, you must configure a client to receive application configuration updates as described in [Step 6: Retrieving the configuration](appconfig-retrieving-the-configuration.md)\. For information about creating a pipeline that specifies AWS AppConfig as the deploy provider, see [Tutorial: Create a Pipeline that Uses AWS AppConfig as a Deployment Provider](https://docs.aws.amazon.com/codepipeline/latest/userguide/tutorials-AppConfig.html) in the *AWS CodePipeline User Guide*\. 

Proceed to [Step 5: Deploying a configuration](appconfig-deploying.md)\.

### Creating an AWS AppConfig deployment strategy \(command line\)<a name="appconfig-creating-deployment-strategy-create-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to create an AWS AppConfig deployment strategy\.

**To create a deployment strategy step by step**

1. Open the AWS CLI\.

1. Run the following command to create a deployment strategy\. 

------
#### [ Linux ]

   ```
   aws appconfig create-deployment-strategy \
     --name A_name_for_the_deployment_strategy \
     --description A_description_of_the_deployment_strategy \
     --deployment-duration-in-minutes Total_amount_of_time_for_a_deployment_to_last \
     --final-bake-time-in-minutes Amount_of_time_AWS AppConfig_monitors_for_alarms_before_considering_the_deployment_to_be_complete \
     --growth-factor The_percentage_of_targets_to_receive_a_deployed_configuration_during_each_interval \
     --growth-type The_linear_or_exponential_algorithm_used_to_define_how_percentage_grows_over_time \
     --replicate-to To_save_the_deployment_strategy_to_a_Systems_Manager_(SSM)_document \
     --tags User_defined_key_value_pair_metadata_of_the_deployment_strategy
   ```

------
#### [ Windows ]

   ```
   aws appconfig create-deployment-strategy ^
     --name A_name_for_the_deployment_strategy ^
     --description A_description_of_the_deployment_strategy ^
     --deployment-duration-in-minutes Total_amount_of_time_for_a_deployment_to_last ^
     --final-bake-time-in-minutes Amount_of_time_AWS AppConfig_monitors_for_alarms_before_considering_the_deployment_to_be_complete ^
     --growth-factor The_percentage_of_targets_to_receive_a_deployed_configuration_during_each_interval ^
     --growth-type The_linear_or_exponential_algorithm_used_to_define_how_percentage_grows_over_time ^
     --name A_name_for_the_deployment_strategy ^
     --replicate-to To_save_the_deployment_strategy_to_a_Systems_Manager_(SSM)_document ^
     --tags User_defined_key_value_pair_metadata_of_the_deployment_strategy
   ```

------
#### [ PowerShell ]

   ```
   New-APPCDeploymentStrategy ` 
     --Name A_name_for_the_deployment_strategy ` 
     --Description A_description_of_the_deployment_strategy `
     --DeploymentDurationInMinutes Total_amount_of_time_for_a_deployment_to_last `
     --FinalBakeTimeInMinutes Amount_of_time_AWS AppConfig_monitors_for_alarms_before_considering_the_deployment_to_be_complete `
     --GrowthFactor The_percentage_of_targets_to_receive_a_deployed_configuration_during_each_interval `
     --GrowthType The_linear_or_exponential_algorithm_used_to_define_how_percentage_grows_over_time `
     --ReplicateTo To_save_the_deployment_strategy_to_a_Systems_Manager_(SSM)_document `
     --Tag Hashtable_type_User_defined_key_value_pair_metadata_of_the_deployment_strategy
   ```

------

   The system returns information like the following\.

------
#### [ Linux ]

   ```
   {
      "Id": "Id of the deployment strategy",
      "Name": "Name of the deployment strategy",
      "Description": "Description of the deployment strategy",
      "DeploymentDurationInMinutes": "Total amount of time the deployment lasted",
      "GrowthType": "The linear or exponential algorithm used to define how percentage grew over time",
      "GrowthFactor": "The percentage of targets that received a deployed configuration during each interval",  
      "FinalBakeTimeInMinutes": "The amount of time AWS AppConfig monitored for alarms before considering the deployment to be complete",
      "ReplicateTo": "The Systems Manager (SSM) document where the deployment strategy is saved"
   }
   ```

------
#### [ Windows ]

   ```
   {
      "Id": "Id of the deployment strategy",
      "Name": "Name of the deployment strategy",
      "Description": "Description of the deployment strategy",
      "DeploymentDurationInMinutes": "Total amount of time the deployment lasted",
      "GrowthType": "The linear or exponential algorithm used to define how percentage grew over time",
      "GrowthFactor": "The percentage of targets that received a deployed configuration during each interval",  
      "FinalBakeTimeInMinutes": "The amount of time AWS AppConfig monitored for alarms before considering the deployment to be complete",
      "ReplicateTo": "The Systems Manager (SSM) document where the deployment strategy is saved"
   }
   ```

------
#### [ PowerShell ]

   ```
   ContentLength               : Runtime of the command
   DeploymentDurationInMinutes : Total amount of time the deployment lasted
   Description                 : Description of the deployment strategy
   FinalBakeTimeInMinutes      : The amount of time AWS AppConfig monitored for alarms before considering the deployment to be complete
   GrowthFactor                : The percentage of targets that received a deployed configuration during each interval
   GrowthType                  : The linear or exponential algorithm used to define how percentage grew over time
   HttpStatusCode              : HTTP Status of the runtime
   Id                          : The deployment strategy ID
   Name                        : Name of the deployment strategy
   ReplicateTo                 : The Systems Manager (SSM) document where the deployment strategy is saved
   ResponseMetadata            : Runtime Metadata
   ```

------