# Step 3: Creating configuration profiles and feature flags<a name="appconfig-creating-configuration-and-profile"></a>

A *configuration* is a collection of settings that influence the behavior of your application\. A *configuration profile* enables AWS AppConfig to access your configuration\. Configuration profiles include the following information\.
+ The URI location where the configuration is stored\. 
+ The AWS Identity and Access Management \(IAM\) role that provides access to the configuration\.
+ A validator for the configuration data\. You can use either a JSON Schema or an AWS Lambda function to validate your configuration profile\. A configuration profile can have a maximum of two validators\.

AWS AppConfig supports the following types of configuration profiles\.
+ [Feature Flags](#feature-flags): Use a feature flag configuration to turn on new features that require a timely deployment, such as a product launch or announcement\.
+ [Freeform configurations](#free-form-configurations): Use a freeform configuration to carefully introduce changes to your application\.

## Feature Flags<a name="feature-flags"></a>

**Note**  
The feature flag configuration profile type is in preview release for AWS AppConfig and is subject to change\.

You can use feature flags to enable or disable features within your applications or to configure different characteristics of your application features using flag attributes\. AWS AppConfig stores feature flag configurations in the AWS AppConfig hosted configuration store in a feature flag format that contains data and metadata about your flags and the flag attributes\. For more information about the AWS AppConfig hosted configuration store, see [About the AWS AppConfig hosted configuration store](#appconfig-creating-configuration-and-profile-about-hosted-store) section\.

**Important**  
To retrieve feature flag configuration data, your application must call the `GetLatestConfiguration` API\. You can't retrieve feature flag configuration data by calling `GetConfiguration`\. For more information, see [GetLatestConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetLatestConfiguration.html) in the *AWS AppConfig API Reference*\.

### Creating a feature flag and a feature flag configuration profile \(console\)<a name="appconfig-creating-feature-flag-configuration-create-console"></a>

Use the following procedure to create an AWS AppConfig feature flag configuration profile and a feature flag configuration by using the AWS AppConfig console\.

**To create a configuration profile**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/appconfig/](https://console.aws.amazon.com/systems-manager/appconfig/)\.

1. On the **Applications** tab, choose the application you created in [Create an AWS AppConfig configuration](appconfig-creating-application.md) and then choose the **Configuration profiles and feature flags** tab\.

1. Choose **Create**\.

1. Choose **Feature flag**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/appconfig/latest/userguide/images/featureflagselect1.png)

**To create a feature flag**

1. On the configuration you created, choose **Add new flag**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/appconfig/latest/userguide/images/addnewflag3.png)

1. Provide a **Flag name** and \(optional\) **Description**\. The **Flag key** auto populates by replacing spaces with underscores in the name you provided\. You can edit the flag key if you want a different value or format\. After the flag is created, you can edit the flag name, but not the flag key\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/appconfig/latest/userguide/images/addnewflagdetails4.png)

1. Specify whether the feature flag is **Enabled** or **Disabled** using the toggle button\.

1. \(Optional\) Add **Attributes** and attribute **Constraints** to the feature flag\. Attributes enable you to provide additional values within your flag\. You can optionally validate attribute values against specified constraints\. Constraints ensure that any unexpected values are not deployed to your application\.

   AWS AppConfig feature flags supports the following types of attributes and their corresponding constraints\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/appconfig/latest/userguide/appconfig-creating-configuration-and-profile.html)
**Note**  
 Select **Required value** to specify whether the attribute value is required\. 

1. Choose **Save new version**\.

Proceed to Step 4: Creating a deployment strategy\.

### Creating a feature flag and a feature flag configuration profile \(commandline\)<a name="appconfig-creating-feature-flag-configuration-commandline"></a>

The following procedure describes how to use the AWS Command Line Interface \(on Linux or Windows\) or Tools for Windows PowerShell to create an AWS AppConfig feature flag configuration profile\.

**To create a feature flags configuration step by step**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Create a feature flag configuration profile specifying its **Type** as `AWS.AppConfig.FeatureFlags`\. The configuration profile must use `hosted` for the location URI\.

------
#### [ Linux ]

   ```
   aws appconfig create-configuration-profile \
     --application-id The_application_ID \
     --name A_name_for_the_configuration_profile \
     --location-uri hosted \
     --type AWS.AppConfig.FeatureFlags \
     --validators Content=JSON_Schema_or_ARN_of_AWS Lambda_function,Type=validators_JSON_SCHEMA_and_LAMBDA
   ```

------
#### [ Windows ]

   ```
   aws appconfig create-configuration-profile ^
     --application-id The_application_ID ^
     --name A_name_for_the_configuration_profile ^
     --location-uri hosted ^
     --type AWS.AppConfig.FeatureFlags ^                  
     --validators Content=JSON_Schema_or_ARN_of_AWS Lambda_function,Type=validators_JSON_SCHEMA_and_LAMBDA
   ```

------
#### [ PowerShell ]

   ```
   New-APPCConfigurationProfile `
     -Name A_name_for_the_configuration_profile `
     -ApplicationId The_application_ID `
     -LocationUri hosted `
     -Type AWS.AppConfig.FeatureFlags `
     -Validators Content=JSON_Schema_or_ARN_of_AWS Lambda_function,Type=validators_JSON_SCHEMA_and_LAMBDA
   ```

------

   The system returns information like the following\.

------
#### [ Linux ]

   ```
   {
      "ApplicationId": "The application ID",
      "Id": "The configuration profile ID",
      "Name": "The name of the configuration profile",
      "LocationUri": "hosted",
      "Type": "AWS.AppConfig.FeatureFlags",
      "Validators": [ 
         { 
            "Content": "The JSON Schema content or the ARN of a Lambda function",
            "Type": "Validators of type JSON_SCHEMA and LAMBDA"
         }
      ]
   }
   ```

------
#### [ Windows ]

   ```
   {
      "ApplicationId": "The application ID",
      "Id": "The configuration profile ID",
      "Name": "The name of the configuration profile",
      "Id": "The configuration profile ID",
      "LocationUri": "hosted",
      "Type": "AWS.AppConfig.FeatureFlags",
      "Validators": [ 
         { 
            "Content": "The JSON Schema content or the ARN of a Lambda function",
            "Type": "Validators of type JSON_SCHEMA and LAMBDA"
         }
      ]
   }
   ```

------
#### [ PowerShell ]

   ```
   ApplicationId    : The application ID
   ContentLength    : Runtime of the command
   HttpStatusCode   : HTTP Status of the runtime
   Id               : The configuration profile ID
   LocationUri      : hosted
   Name             : The name of the configuration profile
   ResponseMetadata : Runtime Metadata
   Type             : AWS.AppConfig.FeatureFlags
   Validators       : {Content: The JSON Schema content or the ARN of a Lambda function, Type : Validators of type JSON_SCHEMA and LAMBDA}
   ```

------

1. Create your feature flag configuration data\. Your data must be in a JSON format and conform to the `AWS.AppConfig.FeatureFlags` JSON schema\. For more information about the schema, see [Type reference for AWS\.AppConfig\.FeatureFlags](#appconfig-type-reference-feature-flags)\.

1. Use the `CreateHostedConfigurationVersion` API to save your feature flag configuration data to AWS AppConfig\.

------
#### [ Linux ]

   ```
   aws appconfig create-hosted-configuration-version \
     --application-id The_application_ID \
     --configuration-profile-id The_configuration_profile_id \
     --content-type "application/json" \
     --content file://path/to/feature_flag_configuration_data \
     service_returned_content_file
   ```

------
#### [ Windows ]

   ```
   aws appconfig create-hosted-configuration-version ^
     --application-id The_application_ID ^
     --configuration-profile-id The_configuration_profile_id ^
     --content-type "application/json" ^
     --content file://path/to/feature_flag_configuration_data ^
     service_returned_content_file
   ```

------
#### [ PowerShell ]

   ```
   New-APPCHostedConfigurationVersion `
     -ApplicationId The_application_ID `
     -ConfigurationProfileId The_configuration_profile_id `
     -ContentType "application/json" `
     -Content file://path/to/feature_flag_configuration_data `
     service_returned_content_file
   ```

------

   The system returns information like the following\.

------
#### [ Linux ]

   ```
   {
      "ApplicationId"          : "The application ID",
      "ConfigurationProfileId" : "The configuration profile ID",
      "VersionNumber"          : "The configuration version number",
      "ContentType"            : "application/json"
   }
   ```

------
#### [ Windows ]

   ```
   {
      "ApplicationId"          : "The application ID",
      "ConfigurationProfileId" : "The configuration profile ID",
      "VersionNumber"          : "The configuration version number",
      "ContentType"            : "application/json"
   }
   ```

------
#### [ PowerShell ]

   ```
   ApplicationId          : The application ID
   ConfigurationProfileId : The configuration profile ID
   VersionNumber          : The configuration version number
   ContentType            : application/json
   ```

------

   The `service_returned_content_file` contains your configuration data that includes some AWS AppConfig generated metadata\.
**Note**  
When you create the hosted configuration version, AWS AppConfig verifies that your data conforms to the `AWS.AppConfig.FeatureFlags` JSON schema\. AWS AppConfig additionally validates that each feature flag attribute in your data satisfies the constraints you defined for those attributes\.

#### Type reference for AWS\.AppConfig\.FeatureFlags<a name="appconfig-type-reference-feature-flags"></a>

Use the `AWS.AppConfig.FeatureFlags` JSON schema as a reference to create your feature flag configuration data\.

```
 {
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "$ref": "#/definitions/flagSetDefinition",
  "additionalProperties": false,
  "definitions": {
    "flagSetDefinition": {
      "type": "object",
      "properties": {
        "version": {
          "$ref": "#/definitions/flagSchemaVersions"
        },
        "flags": {
          "$ref": "#/definitions/flagDefinitions"
        },
        "values": {
          "$ref": "#/definitions/flagValues"
        }
      },
      "required": ["version", "flags"],
      "additionalProperties": false
    },
    "flagDefinitions": {
      "type": "object",
      "patternProperties": {
        "^[a-z][a-zA-Z\\d-_]{0,63}$": {
          "$ref": "#/definitions/flagDefinition"
        }
      },
      "maxProperties": 100,
      "additionalProperties": false
    },
    "flagDefinition": {
      "type": "object",
      "properties": {
        "name": {
          "$ref": "#/definitions/customerDefinedName"
        },
        "description": {
          "$ref": "#/definitions/customerDefinedDescription"
        },
        "_createdAt": {
          "type": "string"
        },
        "_updatedAt": {
          "type": "string"
        },
        "attributes": {
          "$ref": "#/definitions/attributeDefinitions"
        }
      },
      "additionalProperties": false
    },
    "attributeDefinitions": {
      "type": "object",
      "patternProperties": {
        "^[a-z][a-zA-Z\\d-_]{0,63}$": {
          "$ref": "#/definitions/attributeDefinition"
        }
      },
      "maxProperties": 25,
      "additionalProperties": false
    },
    "attributeDefinition": {
      "type": "object",
      "properties": {
        "description": {
          "$ref": "#/definitions/customerDefinedDescription"
        },
        "constraints": {
          "oneOf": [
            { "$ref": "#/definitions/numberConstraints" },
            { "$ref": "#/definitions/stringConstraints" },
            { "$ref": "#/definitions/arrayConstraints" },
            { "$ref": "#/definitions/boolConstraints" }
          ]
        }
      },
      "additionalProperties": false
    },
    "flagValues": {
      "type": "object",
      "patternProperties": {
        "^[a-z][a-zA-Z\\d-_]{0,63}$": {
          "$ref": "#/definitions/flagValue"
        }
      },
      "maxProperties": 100,
      "additionalProperties": false
    },
    "flagValue": {
      "type": "object",
      "properties": {
        "enabled": {
          "type": "boolean"
        },
        "_createdAt": {
          "type": "string"
        },
        "_updatedAt": {
          "type": "string"
        }
      },
      "patternProperties": {
        "^[a-z][a-zA-Z\\d-_]{0,63}$": {
          "$ref": "#/definitions/attributeValue",
          "maxProperties": 25
        }
      },
      "required": ["enabled"],
      "additionalProperties": false
    },
    "attributeValue": {
      "oneOf": [
        { "type": "string", "maxLength": 1024 },
        { "type": "number" },
        { "type": "boolean" },
        {
          "type": "array",
          "oneOf": [
            {
              "items": {
                "type": "string",
                "maxLength": 1024
              }
            },
            {
              "items": {
                "type": "number"
              }
            }
          ]
        }
      ],
      "additionalProperties": false
    },
    "stringConstraints": {
      "type": "object",
      "properties": {
        "type": {
          "type": "string",
          "enum": ["string"]
        },
        "required": {
          "type": "boolean"
        },
        "pattern": {
          "type": "string",
          "maxLength": 1024
        },
        "enum": {
          "type": "array",
          "type": "array",
          "maxLength": 100,
          "items": {
            "oneOf": [
              {
                "type": "string",
                "maxLength": 1024
              },
              {
                "type": "integer"
              }
            ]
          }
        }
      },
      "required": ["type"],
      "not": {
        "required": ["pattern", "enum"]
      },
      "additionalProperties": false
    },
    "numberConstraints": {
      "type": "object",
      "properties": {
        "type": {
          "type": "string",
          "enum": ["number"]
        },
        "required": {
          "type": "boolean"
        },
        "minimum": {
          "type": "integer"
        },
        "maximum": {
          "type": "integer"
        }
      },
      "required": ["type"],
      "additionalProperties": false
    },
    "arrayConstraints": {
      "type": "object",
      "properties": {
        "type": {
          "type": "string",
          "enum": ["array"]
        },
        "required": {
          "type": "boolean"
        },
        "elements": {
          "$ref": "#/definitions/elementConstraints"
        }
      },
      "required": ["type"],
      "additionalProperties": false
    },
    "boolConstraints": {
      "type": "object",
      "properties": {
        "type": {
          "type": "string",
          "enum": ["boolean"]
        },
        "required": {
          "type": "boolean"
        }
      },
      "required": ["type"],
      "additionalProperties": false
    },
    "elementConstraints": {
      "oneOf": [
        { "$ref": "#/definitions/numberConstraints" },
        { "$ref": "#/definitions/stringConstraints" }
      ]
    },
    "customerDefinedName": {
      "type": "string",
      "pattern": "^[^\\n]{1,64}$"
    },
    "customerDefinedDescription": {
      "type": "string",
      "maxLength": 1024
    },
    "flagSchemaVersions": {
      "type": "string",
      "enum": ["1"]
    }
  }
}
```

**Important**  
To retrieve feature flag configuration data, your application must call the `GetLatestConfiguration` API\. You can't retrieve feature flag configuration data by calling `GetConfiguration`\. For more information, see [GetLatestConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetLatestConfiguration.html) in the *AWS AppConfig API Reference*\.

When your application calls [GetLatestConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetLatestConfiguration.html) and receives a newly deployed configuration, the information that defines your feature flags and attributes is removed\. The simplified JSON contains a map of keys that match each of the flag keys you specified\. The simplified JSON also contains mapped values of `true` or `false` for the `enabled` attribute\. If a flag sets `enabled` to `true`, any attributes of the flag will be present as well\. The following JSON schema describes the format of the JSON output\.

```
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "patternProperties": {
    "^[a-z][a-zA-Z\\d-_]{0,63}$": {
      "$ref": "#/definitions/attributeValuesMap"
    }
  },
  "maxProperties": 100,
  "additionalProperties": false,
  "definitions": {
    "attributeValuesMap": {
      "type": "object",
      "properties": {
        "enabled": {
          "type": "boolean"
        }
      },
      "required": ["enabled"],
      "patternProperties": {
        "^[a-z][a-zA-Z\\d-_]{0,63}$": {
          "$ref": "#/definitions/attributeValue"
        }
      },
      "maxProperties": 25,
      "additionalProperties": false
    },
    "attributeValue": {
      "oneOf": [
        { "type": "string","maxLength": 1024 },
        { "type": "number" },
        { "type": "boolean" },
        {
          "type": "array",
          "oneOf": [
            {
              "items": {
                "oneOf": [
                  {
                    "type": "string",
                    "maxLength": 1024
                  }
                ]
              }
            },
            {
              "items": {
                "oneOf": [
                  {
                    "type": "number"
                  }
                ]
              }
            }
          ]
        }
      ],
      "additionalProperties": false
    }
  }
}
```

## Freeform configurations<a name="free-form-configurations"></a>

A freeform configuration profile enables AWS AppConfig to access your configuration from a specified source location\. You can store freeform configurations in the following formats and locations\. 
+ YAML, JSON, or text documents in the AWS AppConfig hosted configuration store\.
+ Objects in an Amazon Simple Storage Service \(Amazon S3\) bucket\.
+ Documents in the Systems Manager document store\.
+ Any integration source action supported by AWS CodePipeline\.

For freeform configurations stored in the AWS AppConfig hosted configuration store or SSM documents, you can create the freeform configuration by using the Systems Manager console at the time you create a configuration profile\. The process is described later in this topic\. 

For freeform configurations stored in SSM parameters or in S3, you must create the parameter or object first and then add it to Parameter Store or S3\. After you create the parameter or object, you can use the procedure in this topic to create the configuration profile\. For information about creating a parameter in Parameter Store, see [Creating Systems Manager parameters](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-su-create.html) in the *AWS Systems Manager User Guide*\. 

### About configuration store quotas and limitations<a name="appconfig-creating-configuration-and-profile-quotas"></a>

AWS AppConfig\-supported configuration store have the following quotas and limitations\.


****  

|  | AWS AppConfig hosted configuration store | S3 | Parameter Store | Document store | AWS CodePipeline | 
| --- | --- | --- | --- | --- | --- | 
|  **Configuration size limit**  | 1 MB |  1 MB Enforced by AWS AppConfig, not S3  |  4 KB \(free tier\) / 8 KB \(advanced parameters\)  |  64 KB  | 1 MBEnforced by AWS AppConfig, not CodePipeline | 
|  **Resource storage limit**  | 1 GB |  Unlimited  |  10,000 parameters \(free tier\) / 100,000 parameters \(advanced parameters\)  |  500 documents  | Limited by the number of configuration profiles per application \(100 profiles per application\) | 
|  **Server\-side encryption**  | Yes |  [SSE\-S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/serv-side-encryption.html)  |  No  |  No  | Yes | 
|  **AWS CloudFormation support**  | Yes |  Not for creating or updating data  |  Yes  |  No  | Yes | 
|  **Validate create or update API actions**  | Not supported |  Not supported  |  Regex supported  |  JSON Schema required for all put and update API actions  | Not supported | 
|  **Pricing**  | Free |  See [Amazon S3 pricing](https://aws.amazon.com//s3/pricing/)  |  See [AWS Systems Manager pricing](https://aws.amazon.com//systems-manager/pricing/)  |  Free  |  See [AWS CodePipeline pricing](https://aws.amazon.com//codepipeline/pricing/)  | 

### About the AWS AppConfig hosted configuration store<a name="appconfig-creating-configuration-and-profile-about-hosted-store"></a>

AWS AppConfig includes an internal or hosted configuration store\. Configurations must be 1 MB or smaller\. The AWS AppConfig hosted configuration store provides the following benefits over other configuration store options\. 
+ You don't need to set up and configure other services such as Amazon Simple Storage Service \(Amazon S3\) or Parameter Store\.
+ You don't need to configure AWS Identity and Access Management \(IAM\) permissions to use the configuration store\.
+ You can store configurations in YAML, JSON, or as text documents\.
+ There is no cost to use the store\.
+ You can create a configuration and add it to the store when you create a configuration profile\.

### Creating a freeform configuration and a freeform configuration profile<a name="appconfig-creating-free-form-configuration-and-profile-create"></a>

**Before you begin**  
Read the following related content before you complete the procedure in this section\.
+ The following procedure requires you to specify an IAM service role so that AWS AppConfig can access your configuration data in the configuration store you choose\. This role is not required if you use the AWS AppConfig hosted configuration store\. If you choose S3, Parameter Store, or the Systems Manager document store, then you must either choose an existing IAM role or choose the option to have the system automatically create the role for you\. For more information, about this role, see [About the configuration profile IAM role](appconfig-creating-configuration-and-profile-iam-role.md)\.
+ If you want to create a configuration profile for configurations stored in S3, you must configure permissions\. For more information about permissions and other requirements for using S3 as a configuration store, see [About configurations stored in Amazon S3](appconfig-creating-configuration-and-profile-S3-source.md)\.
+ If you want to use validators, review the details and requirements for using them\. For more information, see [About validators](appconfig-creating-configuration-and-profile-validators.md)\.

#### Creating an AWS AppConfig freeform configuration profile \(console\)<a name="appconfig-creating-free-form-configuration-and-profile-create-console"></a>

Use the following procedure to create an AWS AppConfig freeform configuration profile and \(optionally\) a freeform\-configuration by using the AWS Systems Manager console\.

**To create a configuration profile**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/appconfig/](https://console.aws.amazon.com/systems-manager/appconfig/)\.

1. On the **Applications** tab, choose the application you created in [Create an AWS AppConfig configuration](appconfig-creating-application.md) and then choose the **Configuration profiles and feature flags** tab\.

1. Choose **Create freeform configuration profile**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/appconfig/latest/userguide/images/freeformselect.png)

1. For **Name**, enter a name for the configuration profile\.

1. For **Description**, enter information about the configuration profile\.

1. On the **Select configuration source** page, choose an option\.

1. 
   + If you selected **AWS AppConfig hosted configuration**, then choose either **YAML**, **JSON**, or **Text**, and enter your configuration in the field\. Choose **Next** and go to Step 10 in this procedure\.
   + If you selected **Amazon S3 object**, then enter the object URI\. Choose **Next**\.
   + If you selected **AWS Systems Manager parameter**, then choose the name of the parameter from the list\. Choose **Next**\.
   + If you selected **AWS CodePipeline**, then choose **Next** and go to Step 10 in this procedure\. 
   + If you selected **AWS Systems Manager document**, then complete the following steps\. 

   1. In the **Document source** section, choose either **Saved document** or **New document**\. 

   1. If you choose **Saved document**, then choose the SSM document from the list\. If you choose **New document**, the **Details** and **Content** sections appear\.

   1. In the **Details** section, enter a name for the new application configuration\.

   1. For the **Application configuration schema** section, either choose the JSON schema using the list or choose **Create schema**\. If you choose **Create schema**, Systems Manager opens the **Create schema** page\. Enter the schema details in the **Content** section, and then choose **Create schema**\.  
![\[Enter details of an AWS AppConfig configuration\]](http://docs.aws.amazon.com/appconfig/latest/userguide/images/appconfig-profile-2.png)

   1. For **Application configuration schema version** either choose the version from the list or choose **Update schema** to edit the schema and create a new version\.

   1. In the **Content** section, choose either **YAML** or **JSON** and then enter the configuration data in the field\.  
![\[Enter configuration data in an AWS AppConfig configuration profile\]](http://docs.aws.amazon.com/appconfig/latest/userguide/images/appconfig-profile-3.png)

   1. Choose **Next**\.

1. In the **Service role** section, choose **New service role** to have AWS AppConfig create the IAM role that provides access to the configuration data\. AWS AppConfig automatically populates the **Role name** field based on the name you entered earlier\. Or, to choose a role that already exists in IAM, choose **Existing service role**\. Choose the role by using the **Role ARN** list\.

1. On the **Add validators** page, choose either **JSON Schema** or **AWS Lambda**\. If you choose **JSON Schema**, enter the JSON Schema in the field\. If you choose **AWS Lambda**, choose the function Amazon Resource Name \(ARN\) and the version from the list\. 
**Important**  
Configuration data stored in SSM documents must validate against an associated JSON Schema before you can add the configuration to the system\. SSM parameters do not require a validation method, but we recommend that you create a validation check for new or updated SSM parameter configurations by using AWS Lambda\.

1. In the **Tags** section, enter a key and an optional value\. You can specify a maximum of 50 tags for a resource\. 

1. Choose **Create configuration profile**\.

**Important**  
If you created a configuration profile for AWS CodePipeline, then after you create a deployment strategy, as described in the next section, you must create a pipeline in CodePipeline that specifies AWS AppConfig as the *deploy provider*\. For information about creating a pipeline that specifies AWS AppConfig as the deploy provider, see [Tutorial: Create a Pipeline That Uses AWS AppConfig as a Deployment Provider](https://docs.aws.amazon.com/codepipeline/latest/userguide/tutorials-AppConfig.html) in the *AWS CodePipeline User Guide*\. 

Proceed to [Step 4: Creating a deployment strategy](appconfig-creating-deployment-strategy.md)\.

#### Creating an AWS AppConfig freeform configuration profile \(commandline\)<a name="appconfig-creating-free-form-configuration-and-profile-create-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to create a AWS AppConfig freeform configuration profile\.

**To create a configuration profile step by step**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create a freeform configuration profile\. 

------
#### [ Linux ]

   ```
   aws appconfig create-configuration-profile \
     --application-id The_application_ID \
     --name A_name_for_the_configuration_profile \
     --description A_description_of_the_configuration_profile \
     --location-uri A_URI_to_locate_the_configuration \
     --retrieval-role-arn The_ARN_of_the_IAM_role_with_permission_to_access_the_configuration_at_the_specified_LocationUri \
     --tags User_defined_key_value_pair_metadata_of_the_configuration_profile \
     --validators "Content=JSON_Schema_content_or_the_ARN_of_an_AWS Lambda_function,Type=validators_of_type_JSON_SCHEMA_and_LAMBDA"
   ```

------
#### [ Windows ]

   ```
   aws appconfig create-configuration-profile ^
     --application-id The_application_ID ^
     --name A_name_for_the_configuration_profile ^
     --description A_description_of_the_configuration_profile ^
     --location-uri A_URI_to_locate_the_configuration ^
     --retrieval-role-arn The_ARN_of_the_IAM_role_with_permission_to_access_the_configuration_at_the_specified_LocationUri ^
     --tags User_defined_key_value_pair_metadata_of_the_configuration_profile ^
     --validators "Content=JSON_Schema_content_or_the_ARN_of_an_AWS Lambda_function,Type=validators_of_type_JSON_SCHEMA_and_LAMBDA"
   ```

------
#### [ PowerShell ]

   ```
   New-APPCConfigurationProfile `
     -Name A_name_for_the_configuration_profile `
     -ApplicationId The_application_ID `
     -Description Description_of_the_configuration_profile `
     -LocationUri A_URI_to_locate_the_configuration `
     -RetrievalRoleArn The_ARN_of_the_IAM_role_with_permission_to_access_the_configuration_at_the_specified_LocationUri `
     -Tag Hashtable_type_user_defined_key_value_pair_metadata_of_the_configuration_profile `
     -Validators "Content=JSON_Schema_content_or_the_ARN_of_an_AWS Lambda_function,Type=validators_of_type_JSON_SCHEMA_and_LAMBDA"
   ```

------

   The system returns information like the following\.

------
#### [ Linux ]

   ```
   {
      "ApplicationId": "The application ID",
      "Id": "The configuration profile ID",
      "Name": "The name of the configuration profile",
      "Description": "The configuration profile description",
      "LocationUri": "The URI location of the configuration",
      "RetrievalRoleArn": "The ARN of an IAM role with permission to access the configuration at the specified LocationUri",
      "Type": "AWS.Freeform"
      "Validators": [ ,
         { 
            "Content": "The JSON Schema content or the ARN of a Lambda function",
            "Type": "Validators of type JSON_SCHEMA and LAMBDA"
         }
      ]
   }
   ```

------
#### [ Windows ]

   ```
   {
      "ApplicationId": "The application ID",
      "Id": "The configuration profile ID",
      "Name": "The name of the configuration profile",
      "Description": "The configuration profile description",
      "Id": "The configuration profile ID",
      "LocationUri": "The URI location of the configuration",
      "RetrievalRoleArn": "The ARN of an IAM role with permission to access the configuration at the specified LocationUri",
      "Type": "AWS.Freeform",
      "Validators": [ 
         { 
            "Content": "The JSON Schema content or the ARN of a Lambda function",
            "Type": "Validators of type JSON_SCHEMA and LAMBDA"
         }
      ]
   }
   ```

------
#### [ PowerShell ]

   ```
   ApplicationId    : The application ID
   ContentLength    : Runtime of the command
   Description      : The configuration profile description
   HttpStatusCode   : HTTP Status of the runtime
   Id               : The configuration profile ID
   LocationUri      : The URI location of the configuration
   Name             : The name of the configuration profile
   ResponseMetadata : Runtime Metadata
   RetrievalRoleArn : The ARN of an IAM role with permission to access the configuration at the specified LocationUri
   Type             : AWS.Freeform
   Validators       : {Content: The JSON Schema content or the ARN of a Lambda function, Type : Validators of type JSON_SCHEMA and LAMBDA}
   ```

------