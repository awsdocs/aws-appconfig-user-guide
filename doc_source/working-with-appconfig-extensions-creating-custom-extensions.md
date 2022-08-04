# Creating a custom AWS AppConfig extension<a name="working-with-appconfig-extensions-creating-custom-extensions"></a>



An extension defines one or more actions that it performs during an AWS AppConfig workflow\. For example, the AWS authored `AWS AppConfig deployment events to Amazon SNS` extension includes an action to send a notification to an Amazon SNS topic\. Each action is invoked either when you interact with AWS AppConfig or when AWS AppConfig is performing a process on your behalf\. These are called *action points*\. AWS AppConfig extensions support the following action points:
+ `PRE_CREATE_HOSTED_CONFIGURATION_VERSION`
+ `PRE_START_DEPLOYMENT`
+ `ON_DEPLOYMENT_START`
+ `ON_DEPLOYMENT_STEP`
+ `ON_DEPLOYMENT_BAKING`
+ `ON_DEPLOYMENT_COMPLETE`
+ `ON_DEPLOYMENT_ROLLED_BACK`

Extension actions configured on `PRE_*` action points are applied after request validation, but before AWS AppConfig performs the activity that corresponds to the action point name\. These action invocations are processed at the same time as a request\. If more than one request is made, action invocations run sequentially\. Also note that `PRE_*` action points receive and can change the contents of a configuration\. `PRE_*` action points can also respond to an error and prevent an action from happening\. 

An extension can also run in parallel with an AWS AppConfig workflow by using an `ON_*` action point\. `ON_*` action points are invoked asynchronously\. `ON_*` action points don't receive the contents of a configuration\. If an extension experiences an error during an `ON_*` action point, the service ignores the error and continues the workflow\.

The following sample extension defines one action that calls the `PRE_CREATE_HOSTED_CONFIGURATION_VERSION` action point\. In the `Uri` field, the action specifies the Amazon Resource Name \(ARN\) of the `MyS3ConfigurationBackUpExtension` Lambda function created earlier in this walkthrough\. The action also specifies the AWS Identity and Access Management \(IAM\) assume role ARN created earlier in this walkthrough\.

**Sample AWS AppConfig extension**

```
{
    "Name": "MySampleExtension",
    "Description": "A sample extension that backs up configurations to an S3 bucket.",
    "Actions": {
        "PRE_CREATE_HOSTED_CONFIGURATION_VERSION": [
            {
                "Name": "PreCreateHostedConfigVersionActionForS3Backup",
                "Uri": "arn:aws:lambda:aws-region:111122223333:function:MyS3ConfigurationBackUpExtension",
                "RoleArn": "arn:aws:iam::111122223333:role/ExtensionsTestRole"
            }
        ]
    },
    "Parameters" : {
        "S3_BUCKET": {
            "Required": false
        }
    }
}
```

**Note**  
To view request syntax and field descriptions when creating an extension, see the [CreateExtension](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_CreateExtension.html) topic in the *AWS AppConfig API Reference*\.

**To create an extension \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/appconfig/](https://console.aws.amazon.com/systems-manager/appconfig/)\.

1. In the navigation pane, choose **AWS AppConfig**\.

1. On the **Extensions** tab, choose **Create extension**\.

1. For **Extension name**, enter a unique name\. For the purposes of this walkthrough, enter **MyS3ConfigurationBackUpExtension**\. Optionally, enter a description\.

1. In the **Actions** section, choose **Add new action**\.

1. For **Action name**, enter a unique name\. For the purposes of this walkthrough, enter **PreCreateHostedConfigVersionActionForS3Backup**\. This name describes the action point used by the action and the extension purpose\.

1. In the **Action point** list, choose **PRE\_CREATE\_HOSTED\_CONFIGURATION\_VERSION**\.

1. For **Uri**, choose **Lambda function** and then choose the function in the **Lambda function** list\. If you don't see your function, verify that you are in the same AWS Region where you created the function\.

1. For **IAM Role**, choose the role you created earlier in this walkthrough\.

1. In the **Extension parameters \(optional\)** section, choose **Add new parameter**\. 

1. For **Parameter name**, enter a name\. For the purposes of this walkthrough, enter **S3\_BUCKET**\.

1. Repeat steps 5–11 to create a second action for the `PRE_START_DEPLOYMENT` action point\.

1. Choose **Create extension**\.