# Creating a Lambda function for a custom AWS AppConfig extension<a name="working-with-appconfig-extensions-creating-custom-lambda"></a>

For most use\-cases, to create a custom extension, you must create an AWS Lambda function to perform any computation and processing defined in the extension\. For information about creating a Lambda function, see [Getting started with Lambda](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html) in the *AWS Lambda Developer Guide*\.

The following sample code for a Lambda function, when invoked, automatically backs up an AWS AppConfig configuration to an Amazon S3 bucket\. The configuration is backed up whenever a new configuration is created or deployed\. The sample uses extension parameters so the bucket name doesn't have to be hardcoded in the Lambda function\. By using extension parameters, the user can attach the extension to multiple applications and back up configurations to different buckets\. The code sample includes comments to further explain the function\.

**Sample Lambda function for an AWS AppConfig extension**

```
from datetime import datetime
import base64
import json

import boto3


def lambda_handler(event, context):
    print(event)
    
    # Extensions that use the PRE_CREATE_HOSTED_CONFIGURATION_VERSION and PRE_START_DEPLOYMENT 
    # action points receive the contents of AWS AppConfig configurations in Lambda event parameters.
    # Configuration contents are received as a base64-encoded string, which the lambda needs to decode 
    # in order to get the configuration data as bytes. For other action points, the content 
    # of the configuration isn't present, so the code below will fail.
    config_data_bytes = base64.b64decode(event["Content"])
    
    # You can specify parameters for extensions. The CreateExtension API action lets you define  
    # which parameters an extension supports. You supply the values for those parameters when you 
    # create an extension association by calling the CreateExtensionAssociation API action.
    # The following code uses a parameter called S3_BUCKET to obtain the value specified in the 
    # extension association. You can specify this parameter when you create the extension 
    # later in this walkthrough.
    extension_association_params = event.get('Parameters', {})
    bucket_name = extension_association_params['S3_BUCKET']
    write_backup_to_s3(bucket_name, config_data_bytes)
    
    # The PRE_CREATE_HOSTED_CONFIGURATION_VERSION and PRE_START_DEPLOYMENT action points can 
    # modify the contents of a configuration. The following code makes a minor change 
    # for the purposes of a demonstration.
    old_config_data_string = config_data_bytes.decode('utf-8')
    new_config_data_string = old_config_data_string.replace('hello', 'hello!')
    new_config_data_bytes = new_config_data_string.encode('utf-8')
    
    # The lambda initially received the configuration data as a base64-encoded string 
    # and must return it in the same format.
    new_config_data_base64string = base64.b64encode(new_config_data_bytes).decode('ascii')
    
    return {
        'statusCode': 200,
        # If you want to modify the contents of the configuration, you must include the new contents in the 
        # Lambda response. If you don't want to modify the contents, you can omit the 'Content' field shown here.
        'Content': new_config_data_base64string
    }


def write_backup_to_s3(bucket_name, config_data_bytes):
    s3 = boto3.resource('s3')
    new_object = s3.Object(bucket_name, f"config_backup_{datetime.now().isoformat()}.txt")
    new_object.put(Body=config_data_bytes)
```

If you want to use this sample during this walkthrough, save it with the name **MyS3ConfigurationBackUpExtension** and copy the Amazon Resource Name \(ARN\) for the function\. You specify the ARN when you create the IAM assume role\. You specify the ARN and the name when you create the extension\.