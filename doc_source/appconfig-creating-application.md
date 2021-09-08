# Step 1: Creating an AWS AppConfig application<a name="appconfig-creating-application"></a>

An application in AWS AppConfig is a logical unit of code that provides capabilities for your customers\. For example, an application can be a microservice that runs on EC2 instances, a mobile application installed by your users, a serverless application using Amazon API Gateway and AWS Lambda, or any system you run on behalf of others\. 

## Creating an AWS AppConfig application \(console\)<a name="appconfig-creating-application-console"></a>

Use the following procedure to create an AWS AppConfig application by using the AWS Systems Manager console\.

**To create an application**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/appconfig/](https://console.aws.amazon.com/systems-manager/appconfig/)\.

1. In the navigation pane choose **AWS AppConfig**\.

1. On the **Applications** tab, choose **Create application**\.

1. For **Name**, enter a name for the application\.

1. For **Description**, enter information about the application\.

1. In the **Tags** section, enter a key and an optional value\. You can specify a maximum of 50 tags for a resource\. 

1. Choose **Create application**\.

AWS AppConfig creates the application and then displays the **Environments** tab\. Proceed to [Step 2: Creating an environment ](appconfig-creating-environment.md)\. You can begin the procedure where it states, "On the **Environments** tab\.\.\."

## Creating an AWS AppConfig application \(commandline\)<a name="appconfig-creating-application-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to create an AWS AppConfig application\.

**To create an application step by step**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create an application\. 

------
#### [ Linux ]

   ```
   aws appconfig create-application \
     --name A_name_for_the_application \
     --description A_description_of_the_application \  
     --tags User_defined_key_value_pair_metadata_for_the_application
   ```

------
#### [ Windows ]

   ```
   aws appconfig create-application ^
     --name A_name_for_the_application ^
     --description A_description_of_the_application ^
     --tags User_defined_key_value_pair_metadata_for_the_application
   ```

------
#### [ PowerShell ]

   ```
   New-APPCApplication `
     -Name Name_for_the_application `
     -Description Description_of_the_application `
     -Tag Hashtable_type_user_defined_key_value_pair_metadata_for_the_application
   ```

------

   The system returns information like the following\.

------
#### [ Linux ]

   ```
   {
      "Id": "Application ID",
      "Name": "Application name",
      "Description": "Description of the application"
   }
   ```

------
#### [ Windows ]

   ```
   {
      "Id": "Application ID",
      "Name": "Application name",
      "Description": "Description of the application"
   }
   ```

------
#### [ PowerShell ]

   ```
   ContentLength    : Runtime of the command
   Description      : Description of the application
   HttpStatusCode   : HTTP Status of the runtime
   Id               : Application ID
   Name             : Application name
   ResponseMetadata : Runtime Metadata
   ```

------