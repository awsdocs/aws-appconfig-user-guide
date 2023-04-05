# Step 3: Creating configuration profiles and feature flags<a name="appconfig-creating-configuration-and-profile"></a>

A *configuration* is a collection of settings that influence the behavior of your application\. A *configuration profile* enables AWS AppConfig to access your configuration\. Configuration profiles include the following information\.
+ The URI location where the configuration is stored\. 
+ The AWS Identity and Access Management \(IAM\) role that provides access to the configuration\.
+ A validator for the configuration data\. You can use either a JSON Schema or an AWS Lambda function to validate your configuration profile\. A configuration profile can have a maximum of two validators\.

AWS AppConfig supports the following types of configuration profiles\.
+ **Feature flag**: Use a feature flag configuration to turn on new features that require a timely deployment, such as a product launch or announcement\.
+ **Freeform**: Use a freeform configuration to carefully introduce changes to your application\.

**Topics**
+ [Example configurations](#appconfig-creating-configuration-and-profile-examples)
+ [About the configuration profile IAM role](#appconfig-creating-configuration-and-profile-iam-role)
+ [About configurations stored in Amazon S3](#appconfig-creating-configuration-and-profile-S3-source)
+ [About validators](#appconfig-creating-configuration-and-profile-validators)
+ [Creating a feature flag configuration profile](#appconfig-creating-configuration-and-profile-feature-flags)
+ [Creating a freeform configuration profile](#appconfig-creating-configuration-and-profile-free-form-configurations)

## Example configurations<a name="appconfig-creating-configuration-and-profile-examples"></a>

Use [AWS AppConfig](http://aws.amazon.com/systems-manager/features/appconfig/), a capability of AWS Systems Manager, to create, manage, and quickly deploy application configurations\. A *configuration* is a collection of settings that influence the behavior of your application\. Here are some examples\.

**Feature flag configuration**

The following feature flag configuration enables or disables mobile payments and default payments on a per\-region basis\.

------
#### [ JSON ]

```
{
  "allow_mobile_payments": {
    "enabled": false
  },
  "default_payments_per_region": {
    "enabled": true
  }
}
```

------
#### [ YAML ]

```
---
allow_mobile_payments:
  enabled: false
default_payments_per_region:
  enabled: true
```

------

**Operational configuration**

The following freeform configuration enforces limits on how an application processes requests\.

------
#### [ JSON ]

```
{
  "throttle-limits": {
    "enabled": "true",
    "throttles": [
      {
        "simultaneous_connections": 12
      },
      {
        "tps_maximum": 5000
      }
    ],
    "limit-background-tasks": [
      true
    ]
  }
}
```

------
#### [ YAML ]

```
---
throttle-limits:
  enabled: 'true'
  throttles:
  - simultaneous_connections: 12
  - tps_maximum: 5000
  limit-background-tasks:
  - true
```

------

**Access control list configuration**

The following access control list freeform configuration specifies which users or groups can access an application\.

------
#### [ JSON ]

```
{
  "allow-list": {
    "enabled": "true",
    "cohorts": [
      {
        "internal_employees": true
      },
      {
        "beta_group": false
      },
      {
        "recent_new_customers": false
      },
      {
        "user_name": "Jane_Doe"
      },
      {
        "user_name": "John_Doe"
      }
    ]
  }
}
```

------
#### [ YAML ]

```
---
allow-list:
  enabled: 'true'
  cohorts:
  - internal_employees: true
  - beta_group: false
  - recent_new_customers: false
  - user_name: Jane_Doe
  - user_name: Ashok_Kumar
```

------

## About the configuration profile IAM role<a name="appconfig-creating-configuration-and-profile-iam-role"></a>

You can create the IAM role that provides access to the configuration data by using AWS AppConfig, as described in the following procedure\. Or you can create the IAM role yourself\. If you create the role by using AWS AppConfig, the system creates the role and specifies one of the following permissions policies, depending on which type of configuration source you choose\.

**Configuration source is a Secrets Manager secret**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "secretsmanager:GetSecretValue"
             ],
            "Resource": [
                "arn:aws:secretsmanager:AWS Region:account_ID:secret:secret_name-a1b2c3"
            ]
        }
    ]
}
```

**Configuration source is a Parameter Store parameter**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameter"
            ],
            "Resource": [
                "arn:aws:ssm:AWS Region:account_ID:parameter/parameter_name"
            ]
        }
    ]
    }
```

**Configuration source is an SSM document**

```
{

    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetDocument"
            ],
            "Resource": [
                "arn:aws:ssm:AWS Region:account_ID:document/document_name"
            ]
        }
    ]
}
```

If you create the role by using AWS AppConfig, the system also creates the following trust relationship for the role\. 

```
{

  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "appconfig.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

## About configurations stored in Amazon S3<a name="appconfig-creating-configuration-and-profile-S3-source"></a>

You can store configurations in an Amazon Simple Storage Service \(Amazon S3\) bucket\. When you create the configuration profile, you specify the URI to a single S3 object in a bucket\. You also specify the Amazon Resource Name \(ARN\) of an AWS Identity and Access Management \(IAM\) role that gives AWS AppConfig permission to get the object\. Before you create a configuration profile for an Amazon S3 object, be aware of the following restrictions\.


****  

| Restriction | Details | 
| --- | --- | 
|  Size  |  Configurations stored as S3 objects can be a maximum of 1 MB in size\.  | 
|  Object encryption  |  A configuration profile can target SSE\-S3 and SSE\-KMS encrypted objects\.  | 
|  Storage classes  |  AWS AppConfig supports the following S3 storage classes: `STANDARD`, `INTELLIGENT_TIERING`, `REDUCED_REDUNDANCY`, `STANDARD_IA`, and `ONEZONE_IA`\. The following classes are not supported: All S3 Glacier classes \(`GLACIER` and `DEEP_ARCHIVE`\)\.  | 
|  Versioning  |  AWS AppConfig requires that the S3 object use versioning\.  | 

### Configuring permissions for a configuration stored as an Amazon S3 object<a name="appconfig-creating-configuration-and-profile-S3-source-permissions"></a>

When you create a configuration profile for a configuration stored as an S3 object, you must specify an ARN for an IAM role that gives AWS AppConfig permission to get the object\. The role must include the following permissions\.

Permissions to access the S3 object
+ s3:GetObject
+ s3:GetObjectVersion

Permissions to list S3 buckets

s3:ListAllMyBuckets

Permissions to access the S3 bucket where the object is stored
+ s3:GetBucketLocation
+ s3:GetBucketVersioning
+ s3:ListBucket
+ s3:ListBucketVersions

Complete the following procedure to create a role that enables AWS AppConfig to get a configuration stored in an S3 object\.

**Creating the IAM Policy for Accessing an S3 Object**  
Use the following procedure to create an IAM policy that enables AWS AppConfig to get a configuration stored in an S3 object\.

**To create an IAM policy for accessing an S3 object**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. On the **Create policy** page, choose the **JSON** tab\.

1. Update the following sample policy with information about your S3 bucket and configuration object\. Then paste the policy into the text field on the **JSON** tab\. 

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "s3:GetObject",
           "s3:GetObjectVersion"
         ],
         "Resource": "arn:aws:s3:::my-bucket/my-configurations/my-configuration.json"
       },
       {
         "Effect": "Allow",
         "Action": [
           "s3:GetBucketLocation",
           "s3:GetBucketVersioning",
           "s3:ListBucketVersions",
           "s3:ListBucket"
         ],
         "Resource": [
           "arn:aws:s3:::my-bucket"
         ]
       },
       {
         "Effect": "Allow",
         "Action": "s3:ListAllMyBuckets",
         "Resource": "*"
       } 
     ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review policy** page, type a name in the **Name** box, and then type a description\.

1. Choose **Create policy**\. The system returns you to the **Roles** page\.

**Creating the IAM Role for Accessing an S3 Object**  
Use the following procedure to create an IAM role that enables AWS AppConfig to get a configuration stored in an S3 object\.

**To create an IAM role for accessing an Amazon S3 object**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Select type of trusted entity** section, choose **AWS service**\.

1. In the **Choose a use case** section, under **Common use cases**, choose **EC2**, and then choose **Next: Permissions**\.

1. On the **Attach permissions policy** page, in the search box, enter the name of the policy you created in the previous procedure\. 

1. Choose the policy and then choose **Next: Tags**\.

1. On the **Add tags \(optional\)** page, enter a key and an optional value, and then choose **Next: Review**\.

1. On the **Review** page, type a name in the **Role name** field, and then type a description\.

1. Choose **Create role**\. The system returns you to the **Roles** page\.

1. On the **Roles** page, choose the role you just created to open the **Summary** page\. Note the **Role Name** and **Role ARN**\. You will specify the role ARN when you create the configuration profile later in this topic\.

**Creating a Trust Relationship**  
Use the following procedure to configure the role you just created to trust AWS AppConfig\.

**To add a trust relationship**

1. In the **Summary** page for the role you just created, choose the **Trust Relationships** tab, and then choose **Edit Trust Relationship**\.

1. Delete `"ec2.amazonaws.com"` and add `"appconfig.amazonaws.com"`, as shown in the following example\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "appconfig.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Choose **Update Trust Policy**\.

## About validators<a name="appconfig-creating-configuration-and-profile-validators"></a>

When you create a configuration and configuration profile, you can specify up to two validators\. A validator ensures that your configuration data is syntactically and semantically correct\. You can create validators in either JSON Schema or as an AWS Lambda function\.

**Important**  
Configuration data stored in SSM documents must validate against an associated JSON Schema before you can add the configuration to the system\. SSM parameters do not require a validation method, but we recommend that you create a validation check for new or updated SSM parameter configurations by using AWS Lambda\.

**JSON Schema Validators**

If you create a configuration in an SSM document, then you must specify or create a JSON Schema for that configuration\. A JSON Schema defines the allowable properties for each application configuration setting\. The JSON Schema functions like a set of rules to ensure that new or updated configuration settings conform to the best practices required by your application\. Here is an example\. 

```
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "title": "$id$",
      "description": "BasicFeatureToggle-1",
      "type": "object",
      "additionalProperties": false,
      "patternProperties": {
          "[^\\s]+$": {
              "type": "boolean"
          }
      },
      "minProperties": 1
    }
```

When you create a configuration from an SSM document, the system automatically verifies that the configuration conforms to the schema requirements\. If it doesn't, AWS AppConfig returns a validation error\.

**Note**  
Note the following information about JSON Schema validators:  
A configuration in an SSM document uses the `ApplicationConfiguration` document type\. The corresponding JSON Schema, uses the `ApplicationConfigurationSchema` document type\.
AWS AppConfig supports JSON Schema version 4\.X for inline schema\. If your application configuration requires a different version of JSON Schema, then you must create a Lambda validator\.

**AWS Lambda Validators**

Lambda function validators must be configured with the following event schema\. AWS AppConfig uses this schema to invoke the Lambda function\. The content is a base64\-encoded string, and the URI is a string\. 

```
{
    "ApplicationId": "The application Id of the configuration profile being validated", 
    "ConfigurationProfileId": "The configuration profile Id of the configuration profile being validated",
    "ConfigurationVersion": "The configuration version of the configuration profile being validated",
    "Content": "Base64EncodedByteString", 
    "Uri": "The uri of the configuration"    
}
```

AWS AppConfig verifies that the Lambda `X-Amz-Function-Error` header is set in the response\. Lambda sets this header if the function throws an exception\. For more information about `X-Amz-Function-Error`, see [Error Handling and Automatic Retries in AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/retries-on-errors.html) in the *AWS Lambda Developer Guide*\.

Here is a simple example of a Lambda response code for a successful validation\.

```
import json

def handler(event, context):
     #Add your validation logic here
     print("We passed!")
```

Here is a simple example of a Lambda response code for an unsuccessful validation\.

```
def handler(event, context):
     #Add your validation logic here
     raise Exception("Failure!")
```

Here is another example that validates only if the configuration parameter is a prime number\.

```
function isPrime(value) {
    if (value < 2) {
        return false;
    }

    for (i = 2; i < value; i++) {
        if (value % i === 0) {
            return false;
        }
    }

    return true;
}

exports.handler = async function(event, context) {
    console.log('EVENT: ' + JSON.stringify(event, null, 2));
    const input = parseInt(Buffer.from(event.content, 'base64').toString('ascii'));
    const prime = isPrime(input);
    console.log('RESULT: ' + input + (prime ? ' is' : ' is not') + ' prime');
    if (!prime) {
        throw input + "is not prime";
    }
}
```

AWS AppConfig calls your validation Lambda when calling the `StartDeployment` and `ValidateConfigurationActivity` API operations\. You must provide `appconfig.amazonaws.com` permissions to invoke your Lambda\. For more information, see [Granting Function Access to AWS Services](https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html#permissions-resource-serviceinvoke)\. AWS AppConfig limits the validation Lambda run time to 15 seconds, including start\-up latency\.

## Creating a feature flag configuration profile<a name="appconfig-creating-configuration-and-profile-feature-flags"></a>

You can use feature flags to enable or disable features within your applications or to configure different characteristics of your application features using flag attributes\. AWS AppConfig stores feature flag configurations in the AWS AppConfig hosted configuration store in a feature flag format that contains data and metadata about your flags and the flag attributes\. For more information about the AWS AppConfig hosted configuration store, see [About the AWS AppConfig hosted configuration store](#appconfig-creating-configuration-and-profile-about-hosted-store) section\.

**Important**  
To retrieve feature flag configuration data, your application must call the `GetLatestConfiguration` API\. You can't retrieve feature flag configuration data by calling `GetConfiguration`, which is deprecated\. For more information, see [GetLatestConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_appconfigdata_GetLatestConfiguration.html) in the *AWS AppConfig API Reference*\.

**Topics**
+ [Creating a feature flag and a feature flag configuration profile \(console\)](#appconfig-creating-feature-flag-configuration-create-console)
+ [Creating a feature flag and a feature flag configuration profile \(command line\)](#appconfig-creating-feature-flag-configuration-commandline)
+ [Type reference for AWS\.AppConfig\.FeatureFlags](#appconfig-type-reference-feature-flags)

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

### Creating a feature flag and a feature flag configuration profile \(command line\)<a name="appconfig-creating-feature-flag-configuration-commandline"></a>

The following procedure describes how to use the AWS Command Line Interface \(on Linux or Windows\) or Tools for Windows PowerShell to create an AWS AppConfig feature flag configuration profile\. If you prefer, you can use AWS CloudShell to run the commands listed below\. For more information, see [What is AWS CloudShell?](https://docs.aws.amazon.com/cloudshell/latest/userguide/welcome.html) in the *AWS CloudShell User Guide*\.

**To create a feature flags configuration step by step**

1. Open the AWS CLI\.

1. Create a feature flag configuration profile specifying its **Type** as `AWS.AppConfig.FeatureFlags`\. The configuration profile must use `hosted` for the location URI\.

------
#### [ Linux ]

   ```
   aws appconfig create-configuration-profile \
     --application-id The_application_ID \
     --name A_name_for_the_configuration_profile \
     --location-uri hosted \
     --type AWS.AppConfig.FeatureFlags
   ```

------
#### [ Windows ]

   ```
   aws appconfig create-configuration-profile ^
     --application-id The_application_ID ^
     --name A_name_for_the_configuration_profile ^
     --location-uri hosted ^
     --type AWS.AppConfig.FeatureFlags
   ```

------
#### [ PowerShell ]

   ```
   New-APPCConfigurationProfile `
     -Name A_name_for_the_configuration_profile `
     -ApplicationId The_application_ID `
     -LocationUri hosted `
     -Type AWS.AppConfig.FeatureFlags
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
     file_name_for_system_to_store_configuration_data
   ```

------
#### [ Windows ]

   ```
   aws appconfig create-hosted-configuration-version ^
     --application-id The_application_ID ^
     --configuration-profile-id The_configuration_profile_id ^
     --content-type "application/json" ^
     --content file://path/to/feature_flag_configuration_data ^
     file_name_for_system_to_store_configuration_data
   ```

------
#### [ PowerShell ]

   ```
   New-APPCHostedConfigurationVersion `
     -ApplicationId The_application_ID `
     -ConfigurationProfileId The_configuration_profile_id `
     -ContentType "application/json" `
     -Content file://path/to/feature_flag_configuration_data `
     file_name_for_system_to_store_configuration_data
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

### Type reference for AWS\.AppConfig\.FeatureFlags<a name="appconfig-type-reference-feature-flags"></a>

Use the `AWS.AppConfig.FeatureFlags` JSON schema as a reference to create your feature flag configuration data\.

```
{
      "$schema": "http://json-schema.org/draft-07/schema#",
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
            "^[a-z][a-zA-Z\d-]{0,63}$": {
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
            "_deprecation": {
              "type": "object",
              "properties": {
                "status": {
                  "type": "string",
                  "enum": ["planned"]
                }
              },
             "additionalProperties": false
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
      },
      "type": "object",
      "$ref": "#/definitions/flagSetDefinition",
      "additionalProperties": false
    }
```

**Important**  
To retrieve feature flag configuration data, your application must call the `GetLatestConfiguration` API\. You can't retrieve feature flag configuration data by calling `GetConfiguration`, which is deprecated\. For more information, see [GetLatestConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetLatestConfiguration.html) in the *AWS AppConfig API Reference*\.

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

## Creating a freeform configuration profile<a name="appconfig-creating-configuration-and-profile-free-form-configurations"></a>

A freeform configuration profile enables AWS AppConfig to access your configuration from a specified source location\. You can store freeform configurations in the following formats and locations: 
+ Configuration data in YAML, JSON, and other formats stored in the AWS AppConfig hosted configuration store
+ Configuration data stored as objects in an Amazon Simple Storage Service \(Amazon S3\) bucket
+ Pipelines stored in AWS CodePipeline
+ Secrets stored in AWS Secrets Manager
+ Standard and secure string parameters stored in AWS Systems Manager Parameter Store
+ Configuration data in SSM documents stored in the Systems Manager document store

For freeform configurations stored in the AWS AppConfig hosted configuration store or SSM documents, you can create the freeform configuration by using the Systems Manager console at the time you create a configuration profile\. The process is described later in this topic\. 

For freeform configurations stored in Parameter Store, Secrets Manager, or S3, you must create the parameter, secret, or object first and store it in the relevant configuration store\. After you store the configuration data, use the procedure in this topic to create the configuration profile\.

**Topics**
+ [About configuration store quotas and limitations](#appconfig-creating-configuration-and-profile-quotas)
+ [About the AWS AppConfig hosted configuration store](#appconfig-creating-configuration-and-profile-about-hosted-store)
+ [Creating a freeform configuration and configuration profile](#appconfig-creating-free-form-configuration-and-profile-create)

### About configuration store quotas and limitations<a name="appconfig-creating-configuration-and-profile-quotas"></a>

Configuration stores supported by AWS AppConfig have the following quotas and limitations\.


****  

|  | AWS AppConfig hosted configuration store | Amazon S3 | Systems Manager Parameter Store | AWS Secrets Manager | Systems Manager Document store | AWS CodePipeline | 
| --- | --- | --- | --- | --- | --- | --- | 
|  **Configuration size limit**  | 1 MB |  1 MB Enforced by AWS AppConfig, not S3  |  4 KB \(free tier\) / 8 KB \(advanced parameters\)  | 64 KB |  64 KB  | 1 MBEnforced by AWS AppConfig, not CodePipeline | 
|  **Resource storage limit**  | 1 GB |  Unlimited  |  10,000 parameters \(free tier\) / 100,000 parameters \(advanced parameters\)  | 500,000 |  500 documents  | Limited by the number of configuration profiles per application \(100 profiles per application\) | 
|  **Server\-side encryption**  | Yes |  [SSE\-S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/serv-side-encryption.html), [SSE\-KMS](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html)  |  Yes  | Yes |  No  | Yes | 
|  **AWS CloudFormation support**  | Yes |  Not for creating or updating data  |  Yes  | Yes |  No  | Yes | 
|  **Validate create or update API operations**  | Not supported |  Not supported  |  Regex supported  | Regex supported |  JSON Schema required for all put and update API operations  | Not supported | 
|  **Pricing**  | Free |  See [Amazon S3 pricing](https://aws.amazon.com//s3/pricing/)  |  See [AWS Systems Manager pricing](https://aws.amazon.com//systems-manager/pricing/)  | See [AWS Secrets Manager pricing](https://aws.amazon.com//secrets-manager/pricing/) |  Free  |  See [AWS CodePipeline pricing](https://aws.amazon.com//codepipeline/pricing/)  | 

### About the AWS AppConfig hosted configuration store<a name="appconfig-creating-configuration-and-profile-about-hosted-store"></a>

AWS AppConfig includes an internal or hosted configuration store\. Configurations must be 1 MB or smaller\. The AWS AppConfig hosted configuration store provides the following benefits over other configuration store options\. 
+ You don't need to set up and configure other services such as Amazon Simple Storage Service \(Amazon S3\) or Parameter Store\.
+ You don't need to configure AWS Identity and Access Management \(IAM\) permissions to use the configuration store\.
+ You can store configurations in YAML, JSON, or as text documents\.
+ There is no cost to use the store\.
+ You can create a configuration and add it to the store when you create a configuration profile\.

### Creating a freeform configuration and configuration profile<a name="appconfig-creating-free-form-configuration-and-profile-create"></a>

This section describes how to create a freeform configuration and configuration profile\. Before you begin, note the following information\.
+ The following procedure requires you to specify an IAM service role so that AWS AppConfig can access your configuration data in the configuration store you choose\. This role is not required if you use the AWS AppConfig hosted configuration store\. If you choose S3, Parameter Store, or the Systems Manager document store, then you must either choose an existing IAM role or choose the option to have the system automatically create the role for you\. For more information, about this role, see [About the configuration profile IAM role](#appconfig-creating-configuration-and-profile-iam-role)\.
+ If you want to create a configuration profile for configurations stored in S3, you must configure permissions\. For more information about permissions and other requirements for using S3 as a configuration store, see [About configurations stored in Amazon S3](#appconfig-creating-configuration-and-profile-S3-source)\.
+ If you want to use validators, review the details and requirements for using them\. For more information, see [About validators](#appconfig-creating-configuration-and-profile-validators)\.

**Topics**
+ [Creating an AWS AppConfig freeform configuration profile \(console\)](#appconfig-creating-free-form-configuration-and-profile-create-console)
+ [Creating an AWS AppConfig freeform configuration profile \(command line\)](#appconfig-creating-free-form-configuration-and-profile-create-commandline)

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
   + If you selected **Secrets Manager secret**, then enter the name of the secret\. Choose **Next**\.
   + If you selected **AWS CodePipeline**, then choose **Next** and go to Step 10 in this procedure\. 
   + If you selected **AWS Systems Manager document**, then complete the following steps\. 

   1. In the **Document source** section, choose either **Saved document** or **New document**\. 

   1. If you choose **Saved document**, then choose the SSM document from the list\. If you choose **New document**, the **Details** and **Content** sections appear\.

   1. In the **Details** section, enter a name for the new application configuration\.

   1. For the **Application configuration schema** section, either choose the JSON schema using the list or choose **Create schema**\. If you choose **Create schema**, Systems Manager opens the **Create schema** page\. Enter the schema details in the **Content** section, and then choose **Create schema**\.  
![\[Enter details of an AWS AppConfig configuration\]](http://docs.aws.amazon.com/appconfig/latest/userguide/images/appconfig-profile-2.png)

   1. For **Application configuration schema version** either choose the version from the list or choose **Update schema** to edit the schema and create a new version\.

   1. In the **Content** section, choose either **YAML** or **JSON** and then enter the configuration data in the field\.

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

#### Creating an AWS AppConfig freeform configuration profile \(command line\)<a name="appconfig-creating-free-form-configuration-and-profile-create-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to create an AWS AppConfig freeform configuration profile\. If you prefer, you can use AWS CloudShell to run the commands listed below\. For more information, see [What is AWS CloudShell?](https://docs.aws.amazon.com/cloudshell/latest/userguide/welcome.html) in the *AWS CloudShell User Guide*\.

**To create a configuration profile step by step**

1. Open the AWS CLI\.

1. Run the following command to create a freeform configuration profile\. 

------
#### [ Linux ]

   ```
   aws appconfig create-configuration-profile \
     --application-id The_application_ID \
     --name A_name_for_the_configuration_profile \
     --description A_description_of_the_configuration_profile \
     --location-uri A_URI_to_locate_the_configuration or hosted \
     --retrieval-role-arn The_ARN_of_the_IAM_role_with_permission_to_access_the_configuration_at_the_specified_LocationUri \
     --tags User_defined_key_value_pair_metadata_of_the_configuration_profile \
     --validators "Content=JSON_Schema_content_or_the_ARN_of_an_AWS Lambda_function,Type=JSON_SCHEMA or LAMBDA"
   ```

------
#### [ Windows ]

   ```
   aws appconfig create-configuration-profile ^
     --application-id The_application_ID ^
     --name A_name_for_the_configuration_profile ^
     --description A_description_of_the_configuration_profile ^
     --location-uri A_URI_to_locate_the_configuration or hosted  ^
     --retrieval-role-arn The_ARN_of_the_IAM_role_with_permission_to_access_the_configuration_at_the_specified_LocationUri ^
     --tags User_defined_key_value_pair_metadata_of_the_configuration_profile ^
     --validators "Content=JSON_Schema_content_or_the_ARN_of_an_AWS Lambda_function,Type=JSON_SCHEMA or LAMBDA"
   ```

------
#### [ PowerShell ]

   ```
   New-APPCConfigurationProfile `
     -Name A_name_for_the_configuration_profile `
     -ApplicationId The_application_ID `
     -Description Description_of_the_configuration_profile `
     -LocationUri A_URI_to_locate_the_configuration or hosted `
     -RetrievalRoleArn The_ARN_of_the_IAM_role_with_permission_to_access_the_configuration_at_the_specified_LocationUri `
     -Tag Hashtable_type_user_defined_key_value_pair_metadata_of_the_configuration_profile `
     -Validators "Content=JSON_Schema_content_or_the_ARN_of_an_AWS Lambda_function,Type=JSON_SCHEMA or LAMBDA"
   ```

------

**Note**  
If you created a configuration in the AWS AppConfig hosted configuration store, you can create new versions of the configuration by using the [CreateHostedConfigurationVersion](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_CreateHostedConfigurationVersion.html) API operations\. To view AWS CLI details and sample commands for this API operation, see [create\-hosted\-configuration\-version](https://docs.aws.amazon.com/cli/latest/reference/appconfig/create-hosted-configuration-version.html) in the *AWS CLI Command Reference*\.