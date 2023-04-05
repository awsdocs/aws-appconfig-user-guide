# Available versions of the AWS AppConfig Lambda extension<a name="appconfig-integration-lambda-extensions-versions"></a>

This topic includes information about AWS AppConfig Lambda extension versions\. The AWS AppConfig Lambda extension supports Lambda functions developed for the x86\-64 and ARM64 \(Graviton2\) platforms\. To work properly, your Lambda function must be configured to use the specific Amazon Resource Name \(ARN\) for the AWS Region where it is currently hosted\. You can view AWS Region and ARN details later in this section\.

**Note**  
AWS AppConfig supports all of the versions listed in [Older extension versions](#appconfig-integration-lambda-extensions-enabling-older-versions)\. We recommend that you periodically update to the latest version to take advantage of extension enhancements\.

**Topics**
+ [AWS AppConfig Lambda Extension release notes](#appconfig-integration-lambda-extensions-versions-release-notes)
+ [Finding your Lambda extension version number](#appconfig-integration-lambda-extensions-versions-find)
+ [x86\-64 platform](#appconfig-integration-lambda-extensions-enabling-x86-64)
+ [ARM64 platform](#appconfig-integration-lambda-extensions-enabling-ARM64)
+ [Older extension versions](#appconfig-integration-lambda-extensions-enabling-older-versions)

## AWS AppConfig Lambda Extension release notes<a name="appconfig-integration-lambda-extensions-versions-release-notes"></a>

The following table describes changes made to recent versions of the AWS AppConfig Lambda extension\.


****  

| Version | Launch date | Notes | 
| --- | --- | --- | 
|  2\.0\.165  |  02/21/2023  |   Minor bug fixes\. No longer restricting extension use to specific runtime versions via the AWS Lambda console\. Added support for the following AWS Regions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/appconfig/latest/userguide/appconfig-integration-lambda-extensions-versions.html)  | 
|  2\.0\.122  |  08/23/2022  | Added support for a tunneling proxy, which can be configured with the `AWS_APPCONFIG_EXTENSION_PROXY_URL` and `AWS_APPCONFIG_EXTENSION_PROXY_HEADERS` environment variables\. Added \.NET 6 as a runtime\. For more information about environment variables, see [Configuring the AWS AppConfig Lambda extension](appconfig-integration-lambda-extensions.md#appconfig-integration-lambda-extensions-config)\.  | 
|  2\.0\.58  |  05/03/2022  |  Improved support for Graviton2 \(ARM64\) processors in Lambda\.  | 
|  2\.0\.45  |  03/15/2022  |  Added support for calling a single feature flag\. Previously, customers called feature flags grouped into a configuration profile and had to parse the response client\-side\. With this release, customers can use a `flag=<flag-name>` parameter when calling the HTTP localhost endpoint to get the value of a single flag\. Also added initial support for Graviton2 \(ARM64\) processors\.  | 

## Finding your Lambda extension version number<a name="appconfig-integration-lambda-extensions-versions-find"></a>

Use the following procedure to locate the version number of your currently configured AWS AppConfig Lambda extension\. To work properly, your Lambda function must be configured to use the specific Amazon Resource Name \(ARN\) for the AWS Region where it is currently hosted\.

1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Choose the Lambda function where you want to add the `AWS-AppConfig-Extension` layer\.

1. In the **Layers** section, choose **Add a layer**\.

1. In the **Choose a layer** section, choose **AWS\-AppConfig\-Extension** from the **AWS layers** list\.

1. Use the **Version** list to choose a version number\.

1. Choose **Add**\.

1. Use the **Test** tab to test the function\.

1. After the test completes, view the log output\. Locate the AWS AppConfig Lambda extension version in the **Details of the Execution** section\. This version must match the required URLs for that version\. 

## x86\-64 platform<a name="appconfig-integration-lambda-extensions-enabling-x86-64"></a>

When you add the extension as a layer to your Lambda, you must specify an ARN\. Choose an ARN from the following table that corresponds with the AWS Region where you created the Lambda\. These ARNs are for Lambda functions developed for the x86\-64 platform\.


**Version 2\.0\.165**  

| Region | ARN | 
| --- | --- | 
|  US East \(N\. Virginia\)  |  `arn:aws:lambda:us-east-1:027255383542:layer:AWS-AppConfig-Extension:110`  | 
|  US East \(Ohio\)  |  `arn:aws:lambda:us-east-2:728743619870:layer:AWS-AppConfig-Extension:79`  | 
|  US West \(N\. California\)  |  `arn:aws:lambda:us-west-1:958113053741:layer:AWS-AppConfig-Extension:121`  | 
|  US West \(Oregon\)  |  `arn:aws:lambda:us-west-2:359756378197:layer:AWS-AppConfig-Extension:143`  | 
|  Canada \(Central\)  |  `arn:aws:lambda:ca-central-1:039592058896:layer:AWS-AppConfig-Extension:79`  | 
|  Europe \(Frankfurt\)  |  `arn:aws:lambda:eu-central-1:066940009817:layer:AWS-AppConfig-Extension:91`  | 
|  Europe \(Zurich\)  |  `arn:aws:lambda:eu-central-2:758369105281:layer:AWS-AppConfig-Extension:29`  | 
|  Europe \(Ireland\)  |  `arn:aws:lambda:eu-west-1:434848589818:layer:AWS-AppConfig-Extension:108`  | 
|  Europe \(London\)  |  `arn:aws:lambda:eu-west-2:282860088358:layer:AWS-AppConfig-Extension:79`  | 
|  Europe \(Paris\)  |  `arn:aws:lambda:eu-west-3:493207061005:layer:AWS-AppConfig-Extension:80`  | 
|  Europe \(Stockholm\)  |  `arn:aws:lambda:eu-north-1:646970417810:layer:AWS-AppConfig-Extension:139`  | 
|  Europe \(Milan\)  |  `arn:aws:lambda:eu-south-1:203683718741:layer:AWS-AppConfig-Extension:71`  | 
|  Europe \(Spain\)  |  `arn:aws:lambda:eu-south-2:586093569114:layer:AWS-AppConfig-Extension:26`  | 
| China \(Beijing\) |  `arn:aws-cn:lambda:cn-north-1:615057806174:layer:AWS-AppConfig-Extension:66`  | 
| China \(Ningxia\) |  `arn:aws-cn:lambda:cn-northwest-1:615084187847:layer:AWS-AppConfig-Extension:66`  | 
|  Asia Pacific \(Hong Kong\)  |  `arn:aws:lambda:ap-east-1:630222743974:layer:AWS-AppConfig-Extension:71`  | 
|  Asia Pacific \(Tokyo\)  |  `arn:aws:lambda:ap-northeast-1:980059726660:layer:AWS-AppConfig-Extension:82`  | 
|  Asia Pacific \(Seoul\)  |  `arn:aws:lambda:ap-northeast-2:826293736237:layer:AWS-AppConfig-Extension:91`  | 
| Asia Pacific \(Osaka\) |  `arn:aws:lambda:ap-northeast-3:706869817123:layer:AWS-AppConfig-Extension:84`  | 
|  Asia Pacific \(Singapore\)  |  `arn:aws:lambda:ap-southeast-1:421114256042:layer:AWS-AppConfig-Extension:89`  | 
|  Asia Pacific \(Sydney\)  |  `arn:aws:lambda:ap-southeast-2:080788657173:layer:AWS-AppConfig-Extension:91`  | 
|  Asia Pacific \(Jakarta\)  |  `arn:aws:lambda:ap-southeast-3:418787028745:layer:AWS-AppConfig-Extension:60`  | 
|  Asia Pacific \(Melbourne\)  |  `arn:aws:lambda:ap-southeast-4:307021474294:layer:AWS-AppConfig-Extension:2`  | 
|  Asia Pacific \(Mumbai\)  |  `arn:aws:lambda:ap-south-1:554480029851:layer:AWS-AppConfig-Extension:92`  | 
|  Asia Pacific \(Hyderabad\)  |  `arn:aws:lambda:ap-south-2:489524808438:layer:AWS-AppConfig-Extension:29`  | 
|  South America \(São Paulo\)  |  `arn:aws:lambda:sa-east-1:000010852771:layer:AWS-AppConfig-Extension:110`  | 
|  Africa \(Cape Town\)  |  `arn:aws:lambda:af-south-1:574348263942:layer:AWS-AppConfig-Extension:71`  | 
|  Middle East \(UAE\)  |  `arn:aws:lambda:me-central-1:662846165436:layer:AWS-AppConfig-Extension:31`  | 
|  Middle East \(Bahrain\)  |  `arn:aws:lambda:me-south-1:559955524753:layer:AWS-AppConfig-Extension:71`  | 
| AWS GovCloud \(US\-East\) |  `arn:aws-us-gov:lambda:us-gov-east-1:946561847325:layer:AWS-AppConfig-Extension:44`  | 
| AWS GovCloud \(US\-West\) |  `arn:aws-us-gov:lambda:us-gov-west-1:946746059096:layer:AWS-AppConfig-Extension:44`  | 

## ARM64 platform<a name="appconfig-integration-lambda-extensions-enabling-ARM64"></a>

When you add the extension as a layer to your Lambda, you must specify an ARN\. Choose an ARN from the following table that corresponds with the AWS Region where you created the Lambda\. These ARNs are for Lambda functions developed for the ARM64 platform\.


**Version 2\.0\.165**  

| Region | ARN | 
| --- | --- | 
|  US East \(N\. Virginia\)  |  `arn:aws:lambda:us-east-1:027255383542:layer:AWS-AppConfig-Extension-Arm64:43`  | 
|  US East \(Ohio\)  |  `arn:aws:lambda:us-east-2:728743619870:layer:AWS-AppConfig-Extension-Arm64:31`  | 
|  US West \(Oregon\)  |  `arn:aws:lambda:us-west-2:359756378197:layer:AWS-AppConfig-Extension-Arm64:45`  | 
|  Europe \(Frankfurt\)  |  `arn:aws:lambda:eu-central-1:066940009817:layer:AWS-AppConfig-Extension-Arm64:34`  | 
|  Europe \(Ireland\)  |  `arn:aws:lambda:eu-west-1:434848589818:layer:AWS-AppConfig-Extension-Arm64:46`  | 
|  Europe \(London\)  |  `arn:aws:lambda:eu-west-2:282860088358:layer:AWS-AppConfig-Extension-Arm64:31`  | 
|  Asia Pacific \(Tokyo\)  |  `arn:aws:lambda:ap-northeast-1:980059726660:layer:AWS-AppConfig-Extension-Arm64:35`  | 
|  Asia Pacific \(Singapore\)  |  `arn:aws:lambda:ap-southeast-1:421114256042:layer:AWS-AppConfig-Extension-Arm64:41`  | 
|  Asia Pacific \(Sydney\)  |  `arn:aws:lambda:ap-southeast-2:080788657173:layer:AWS-AppConfig-Extension-Arm64:34`  | 
|  Asia Pacific \(Mumbai\)  |  `arn:aws:lambda:ap-south-1:554480029851:layer:AWS-AppConfig-Extension-Arm64:34`  | 

## Older extension versions<a name="appconfig-integration-lambda-extensions-enabling-older-versions"></a>

This section lists the ARNs and AWS Regions for older versions of the AWS AppConfig Lambda extension\. This list doesn't contain information for all previous versions of the AWS AppConfig Lambda extension, but it will be updated when new versions are released\.

### Older extension versions \(x86\-64 platform\)<a name="appconfig-integration-lambda-extensions-enabling-older-versions-x86-64"></a>

The following tables list ARNs and the AWS Regions for older versions of the AWS AppConfig Lambda extension developed for the x86\-64 platform\.

Date replaced by newer extension: 02/21/2023


**Version 2\.0\.122**  

| Region | ARN | 
| --- | --- | 
|  US East \(N\. Virginia\)   |  `arn:aws:lambda:us-east-1:027255383542:layer:AWS-AppConfig-Extension:82`  | 
|  US East \(Ohio\)  |  `arn:aws:lambda:us-east-2:728743619870:layer:AWS-AppConfig-Extension:59`  | 
|  US West \(N\. California\)  |  `arn:aws:lambda:us-west-1:958113053741:layer:AWS-AppConfig-Extension:93`  | 
|  US West \(Oregon\)  |  `arn:aws:lambda:us-west-2:359756378197:layer:AWS-AppConfig-Extension:114`  | 
|  Canada \(Central\)  |  `arn:aws:lambda:ca-central-1:039592058896:layer:AWS-AppConfig-Extension:59`  | 
|  Europe \(Frankfurt\)  |  `arn:aws:lambda:eu-central-1:066940009817:layer:AWS-AppConfig-Extension:70`  | 
|  Europe \(Ireland\)  |  `arn:aws:lambda:eu-west-1:434848589818:layer:AWS-AppConfig-Extension:82`  | 
|  Europe \(London\)  |  `arn:aws:lambda:eu-west-2:282860088358:layer:AWS-AppConfig-Extension:59`  | 
|  Europe \(Paris\)  |  `arn:aws:lambda:eu-west-3:493207061005:layer:AWS-AppConfig-Extension:60`  | 
|  Europe \(Stockholm\)  |  `arn:aws:lambda:eu-north-1:646970417810:layer:AWS-AppConfig-Extension:111`  | 
|  Europe \(Milan\)  |  `arn:aws:lambda:eu-south-1:203683718741:layer:AWS-AppConfig-Extension:54`  | 
| China \(Beijing\) |  `arn:aws-cn:lambda:cn-north-1:615057806174:layer:AWS-AppConfig-Extension:52`  | 
| China \(Ningxia\) |  `arn:aws-cn:lambda:cn-northwest-1:615084187847:layer:AWS-AppConfig-Extension:52`  | 
|  Asia Pacific \(Hong Kong\)  |  `arn:aws:lambda:ap-east-1:630222743974:layer:AWS-AppConfig-Extension:54`  | 
|  Asia Pacific \(Tokyo\)  |  `arn:aws:lambda:ap-northeast-1:980059726660:layer:AWS-AppConfig-Extension:62`  | 
|  Asia Pacific \(Seoul\)  |  `arn:aws:lambda:ap-northeast-2:826293736237:layer:AWS-AppConfig-Extension:70`  | 
| Asia Pacific \(Osaka\) |  `arn:aws:lambda:ap-northeast-3:706869817123:layer:AWS-AppConfig-Extension:59`  | 
|  Asia Pacific \(Singapore\)  |  `arn:aws:lambda:ap-southeast-1:421114256042:layer:AWS-AppConfig-Extension:64`  | 
|  Asia Pacific \(Sydney\)  |  `arn:aws:lambda:ap-southeast-2:080788657173:layer:AWS-AppConfig-Extension:70`  | 
|  Asia Pacific \(Jakarta\)  |  `arn:aws:lambda:ap-southeast-3:418787028745:layer:AWS-AppConfig-Extension:37`  | 
|  Asia Pacific \(Mumbai\)  |  `arn:aws:lambda:ap-south-1:554480029851:layer:AWS-AppConfig-Extension:71`  | 
|  South America \(São Paulo\)  |  `arn:aws:lambda:sa-east-1:000010852771:layer:AWS-AppConfig-Extension:82`  | 
|  Africa \(Cape Town\)  |  `arn:aws:lambda:af-south-1:574348263942:layer:AWS-AppConfig-Extension:54`  | 
|  Middle East \(Bahrain\)  |  `arn:aws:lambda:me-south-1:559955524753:layer:AWS-AppConfig-Extension:54`  | 
| AWS GovCloud \(US\-East\) |  `arn:aws-us-gov:lambda:us-gov-east-1:946561847325:layer:AWS-AppConfig-Extension:29`  | 
| AWS GovCloud \(US\-West\) |  `arn:aws-us-gov:lambda:us-gov-west-1:946746059096:layer:AWS-AppConfig-Extension:29`  | 

Date replaced by newer extension: 08/23/2022


**Version 2\.0\.58**  

| Region | ARN | 
| --- | --- | 
|  US East \(N\. Virginia\)   |  `arn:aws:lambda:us-east-1:027255383542:layer:AWS-AppConfig-Extension:69`  | 
|  US East \(Ohio\)  |  `arn:aws:lambda:us-east-2:728743619870:layer:AWS-AppConfig-Extension:50`  | 
|  US West \(N\. California\)  |  `arn:aws:lambda:us-west-1:958113053741:layer:AWS-AppConfig-Extension:78`  | 
|  US West \(Oregon\)  |  `arn:aws:lambda:us-west-2:359756378197:layer:AWS-AppConfig-Extension:101`  | 
|  Canada \(Central\)  |  `arn:aws:lambda:ca-central-1:039592058896:layer:AWS-AppConfig-Extension:50`  | 
|  Europe \(Frankfurt\)  |  `arn:aws:lambda:eu-central-1:066940009817:layer:AWS-AppConfig-Extension:59`  | 
|  Europe \(Ireland\)  |  `arn:aws:lambda:eu-west-1:434848589818:layer:AWS-AppConfig-Extension:69`  | 
|  Europe \(London\)  |  `arn:aws:lambda:eu-west-2:282860088358:layer:AWS-AppConfig-Extension:50`  | 
|  Europe \(Paris\)  |  `arn:aws:lambda:eu-west-3:493207061005:layer:AWS-AppConfig-Extension:51`  | 
|  Europe \(Stockholm\)  |  `arn:aws:lambda:eu-north-1:646970417810:layer:AWS-AppConfig-Extension:98`  | 
|  Europe \(Milan\)  |  `arn:aws:lambda:eu-south-1:203683718741:layer:AWS-AppConfig-Extension:47`  | 
| China \(Beijing\) |  `arn:aws-cn:lambda:cn-north-1:615057806174:layer:AWS-AppConfig-Extension:46`  | 
| China \(Ningxia\) |  `arn:aws-cn:lambda:cn-northwest-1:615084187847:layer:AWS-AppConfig-Extension:46`  | 
|  Asia Pacific \(Hong Kong\)  |  `arn:aws:lambda:ap-east-1:630222743974:layer:AWS-AppConfig-Extension:47`  | 
|  Asia Pacific \(Tokyo\)  |  `arn:aws:lambda:ap-northeast-1:980059726660:layer:AWS-AppConfig-Extension:49`  | 
|  Asia Pacific \(Seoul\)  |  `arn:aws:lambda:ap-northeast-2:826293736237:layer:AWS-AppConfig-Extension:59`  | 
| Asia Pacific \(Osaka\) |  `arn:aws:lambda:ap-northeast-3:706869817123:layer:AWS-AppConfig-Extension:46`  | 
|  Asia Pacific \(Singapore\)  |  `arn:aws:lambda:ap-southeast-1:421114256042:layer:AWS-AppConfig-Extension:51`  | 
|  Asia Pacific \(Sydney\)  |  `arn:aws:lambda:ap-southeast-2:080788657173:layer:AWS-AppConfig-Extension:59`  | 
|  Asia Pacific \(Jakarta\)  |  `arn:aws:lambda:ap-southeast-3:418787028745:layer:AWS-AppConfig-Extension:24`  | 
|  Asia Pacific \(Mumbai\)  |  `arn:aws:lambda:ap-south-1:554480029851:layer:AWS-AppConfig-Extension:60`  | 
|  South America \(São Paulo\)  |  `arn:aws:lambda:sa-east-1:000010852771:layer:AWS-AppConfig-Extension:69`  | 
|  Africa \(Cape Town\)  |  `arn:aws:lambda:af-south-1:574348263942:layer:AWS-AppConfig-Extension:47`  | 
|  Middle East \(Bahrain\)  |  `arn:aws:lambda:me-south-1:559955524753:layer:AWS-AppConfig-Extension:47`  | 
| AWS GovCloud \(US\-East\) |  `arn:aws-us-gov:lambda:us-gov-east-1:946561847325:layer:AWS-AppConfig-Extension:23`  | 
| AWS GovCloud \(US\-West\) |  `arn:aws-us-gov:lambda:us-gov-west-1:946746059096:layer:AWS-AppConfig-Extension:23`  | 

Date replaced by newer extension: 04/21/2022


**Version 2\.0\.45**  

| Region | ARN | 
| --- | --- | 
|  US East \(N\. Virginia\)  |  `arn:aws:lambda:us-east-1:027255383542:layer:AWS-AppConfig-Extension:68`  | 
|  US East \(Ohio\)  |  `arn:aws:lambda:us-east-2:728743619870:layer:AWS-AppConfig-Extension:49`  | 
|  US West \(N\. California\)  |  `arn:aws:lambda:us-west-1:958113053741:layer:AWS-AppConfig-Extension:77`  | 
|  US West \(Oregon\)  |  `arn:aws:lambda:us-west-2:359756378197:layer:AWS-AppConfig-Extension:100`  | 
|  Canada \(Central\)  |  `arn:aws:lambda:ca-central-1:039592058896:layer:AWS-AppConfig-Extension:49`  | 
|  Europe \(Frankfurt\)  |  `arn:aws:lambda:eu-central-1:066940009817:layer:AWS-AppConfig-Extension:58`  | 
|  Europe \(Ireland\)  |  `arn:aws:lambda:eu-west-1:434848589818:layer:AWS-AppConfig-Extension:68`  | 
|  Europe \(London\)  |  `arn:aws:lambda:eu-west-2:282860088358:layer:AWS-AppConfig-Extension:49`  | 
|  Europe \(Paris\)  |  `arn:aws:lambda:eu-west-3:493207061005:layer:AWS-AppConfig-Extension:50`  | 
|  Europe \(Stockholm\)  |  `arn:aws:lambda:eu-north-1:646970417810:layer:AWS-AppConfig-Extension:97`  | 
|  Europe \(Milan\)  |  `arn:aws:lambda:eu-south-1:203683718741:layer:AWS-AppConfig-Extension:46`  | 
| China \(Beijing\) |  `arn:aws-cn:lambda:cn-north-1:615057806174:layer:AWS-AppConfig-Extension:45`  | 
| China \(Ningxia\) |  `arn:aws-cn:lambda:cn-northwest-1:615084187847:layer:AWS-AppConfig-Extension:45`  | 
|  Asia Pacific \(Hong Kong\)  |  `arn:aws:lambda:ap-east-1:630222743974:layer:AWS-AppConfig-Extension:46`  | 
|  Asia Pacific \(Tokyo\)  |  `arn:aws:lambda:ap-northeast-1:980059726660:layer:AWS-AppConfig-Extension:48`  | 
|  Asia Pacific \(Seoul\)  |  `arn:aws:lambda:ap-northeast-2:826293736237:layer:AWS-AppConfig-Extension:58`  | 
| Asia Pacific \(Osaka\) |  `arn:aws:lambda:ap-northeast-3:706869817123:layer:AWS-AppConfig-Extension:45`  | 
|  Asia Pacific \(Singapore\)  |  `arn:aws:lambda:ap-southeast-1:421114256042:layer:AWS-AppConfig-Extension:50`  | 
|  Asia Pacific \(Sydney\)  |  `arn:aws:lambda:ap-southeast-2:080788657173:layer:AWS-AppConfig-Extension:58`  | 
|  Asia Pacific \(Jakarta\)  |  `arn:aws:lambda:ap-southeast-3:418787028745:layer:AWS-AppConfig-Extension:23`  | 
|  Asia Pacific \(Mumbai\)  |  `arn:aws:lambda:ap-south-1:554480029851:layer:AWS-AppConfig-Extension:59`  | 
|  South America \(São Paulo\)  |  `arn:aws:lambda:sa-east-1:000010852771:layer:AWS-AppConfig-Extension:68`  | 
|  Africa \(Cape Town\)  |  `arn:aws:lambda:af-south-1:574348263942:layer:AWS-AppConfig-Extension:46`  | 
|  Middle East \(Bahrain\)  |  `arn:aws:lambda:me-south-1:559955524753:layer:AWS-AppConfig-Extension:46`  | 
| AWS GovCloud \(US\-East\) |  `arn:aws-us-gov:lambda:us-gov-east-1:946561847325:layer:AWS-AppConfig-Extension:22`  | 
| AWS GovCloud \(US\-West\) |  `arn:aws-us-gov:lambda:us-gov-west-1:946746059096:layer:AWS-AppConfig-Extension:22`  | 

Date replaced by newer extension: 03/15/2022


**Version 2\.0\.30**  

| Region | ARN | 
| --- | --- | 
|  US East \(N\. Virginia\)  |  `arn:aws:lambda:us-east-1:027255383542:layer:AWS-AppConfig-Extension:61`  | 
|  US East \(Ohio\)  |  `arn:aws:lambda:us-east-2:728743619870:layer:AWS-AppConfig-Extension:47`  | 
|  US West \(N\. California\)  |  `arn:aws:lambda:us-west-1:958113053741:layer:AWS-AppConfig-Extension:61`  | 
|  US West \(Oregon\)  |  `arn:aws:lambda:us-west-2:359756378197:layer:AWS-AppConfig-Extension:89`  | 
|  Canada \(Central\)  |  `arn:aws:lambda:ca-central-1:039592058896:layer:AWS-AppConfig-Extension:47`  | 
|  Europe \(Frankfurt\)  |  `arn:aws:lambda:eu-central-1:066940009817:layer:AWS-AppConfig-Extension:54`  | 
|  Europe \(Ireland\)  |  `arn:aws:lambda:eu-west-1:434848589818:layer:AWS-AppConfig-Extension:59`  | 
|  Europe \(London\)  |  `arn:aws:lambda:eu-west-2:282860088358:layer:AWS-AppConfig-Extension:47`  | 
|  Europe \(Paris\)  |  `arn:aws:lambda:eu-west-3:493207061005:layer:AWS-AppConfig-Extension:48`  | 
|  Europe \(Stockholm\)  |  `arn:aws:lambda:eu-north-1:646970417810:layer:AWS-AppConfig-Extension:86`  | 
|  Europe \(Milan\)  |  `arn:aws:lambda:eu-south-1:203683718741:layer:AWS-AppConfig-Extension:44`  | 
| China \(Beijing\) |  `arn:aws-cn:lambda:cn-north-1:615057806174:layer:AWS-AppConfig-Extension:43`  | 
| China \(Ningxia\) |  `arn:aws-cn:lambda:cn-northwest-1:615084187847:layer:AWS-AppConfig-Extension:43`  | 
|  Asia Pacific \(Hong Kong\)  |  `arn:aws:lambda:ap-east-1:630222743974:layer:AWS-AppConfig-Extension:44`  | 
|  Asia Pacific \(Tokyo\)  |  `arn:aws:lambda:ap-northeast-1:980059726660:layer:AWS-AppConfig-Extension:45`  | 
| Asia Pacific \(Osaka\) |  `arn:aws:lambda:ap-northeast-3:706869817123:layer:AWS-AppConfig-Extension:42`  | 
|  Asia Pacific \(Seoul\)  |  `arn:aws:lambda:ap-northeast-2:826293736237:layer:AWS-AppConfig-Extension:54`  | 
|  Asia Pacific \(Singapore\)  |  `arn:aws:lambda:ap-southeast-1:421114256042:layer:AWS-AppConfig-Extension:45`  | 
|  Asia Pacific \(Sydney\)  |  `arn:aws:lambda:ap-southeast-2:080788657173:layer:AWS-AppConfig-Extension:54`  | 
|  Asia Pacific \(Jakarta\)  |  `arn:aws:lambda:ap-southeast-3:418787028745:layer:AWS-AppConfig-Extension:13`  | 
|  Asia Pacific \(Mumbai\)  |  `arn:aws:lambda:ap-south-1:554480029851:layer:AWS-AppConfig-Extension:55`  | 
|  South America \(São Paulo\)  |  `arn:aws:lambda:sa-east-1:000010852771:layer:AWS-AppConfig-Extension:61`  | 
|  Africa \(Cape Town\)  |  `arn:aws:lambda:af-south-1:574348263942:layer:AWS-AppConfig-Extension:44`  | 
|  Middle East \(Bahrain\)  |  `arn:aws:lambda:me-south-1:559955524753:layer:AWS-AppConfig-Extension:44`  | 
| AWS GovCloud \(US\-East\) |  `arn:aws-us-gov:lambda:us-gov-east-1:946561847325:layer:AWS-AppConfig-Extension:20`  | 
| AWS GovCloud \(US\-West\) |  `arn:aws-us-gov:lambda:us-gov-west-1:946746059096:layer:AWS-AppConfig-Extension:20`  | 

### Older extension versions \(ARM64 platform\)<a name="appconfig-integration-lambda-extensions-enabling-older-versions-ARM64"></a>

The following tables list ARNs and the AWS Regions for older versions of the AWS AppConfig Lambda extension developed for the ARM64 platform\.

Date replaced by newer extension: 02/21/2023


**Version 2\.0\.122**  

| Region | ARN | 
| --- | --- | 
|  US East \(N\. Virginia\)   |  `arn:aws:lambda:us-east-1:027255383542:layer:AWS-AppConfig-Extension-Arm64:15`  | 
|  US East \(Ohio\)  |  `arn:aws:lambda:us-east-2:728743619870:layer:AWS-AppConfig-Extension-Arm64:11`  | 
|  US West \(Oregon\)  |  `arn:aws:lambda:us-west-2:359756378197:layer:AWS-AppConfig-Extension-Arm64:16`  | 
|  Europe \(Frankfurt\)  |  `arn:aws:lambda:eu-central-1:066940009817:layer:AWS-AppConfig-Extension-Arm64:13`  | 
|  Europe \(Ireland\)  |  `arn:aws:lambda:eu-west-1:434848589818:layer:AWS-AppConfig-Extension-Arm64:20`  | 
|  Europe \(London\)  |  `arn:aws:lambda:eu-west-2:282860088358:layer:AWS-AppConfig-Extension-Arm64:11`  | 
|  Asia Pacific \(Tokyo\)  |  `arn:aws:lambda:ap-northeast-1:980059726660:layer:AWS-AppConfig-Extension-Arm64:15`  | 
|  Asia Pacific \(Singapore\)  |  `arn:aws:lambda:ap-southeast-1:421114256042:layer:AWS-AppConfig-Extension-Arm64:16`  | 
|  Asia Pacific \(Sydney\)  |  `arn:aws:lambda:ap-southeast-2:080788657173:layer:AWS-AppConfig-Extension-Arm64:13`  | 
|  Asia Pacific \(Mumbai\)  |  `arn:aws:lambda:ap-south-1:554480029851:layer:AWS-AppConfig-Extension-Arm64:13`  | 

Date replaced by newer extension: 08/23/2022


**Version 2\.0\.58**  

| Region | ARN | 
| --- | --- | 
|  US East \(N\. Virginia\)  |  `arn:aws:lambda:us-east-1:027255383542:layer:AWS-AppConfig-Extension-Arm64:2`  | 
|  US East \(Ohio\)  |  `arn:aws:lambda:us-east-2:728743619870:layer:AWS-AppConfig-Extension-Arm64:2`  | 
|  US West \(Oregon\)  |  `arn:aws:lambda:us-west-2:359756378197:layer:AWS-AppConfig-Extension-Arm64:3`  | 
|  Europe \(Frankfurt\)  |  `arn:aws:lambda:eu-central-1:066940009817:layer:AWS-AppConfig-Extension-Arm64:2`  | 
|  Europe \(Ireland\)  |  `arn:aws:lambda:eu-west-1:434848589818:layer:AWS-AppConfig-Extension-Arm64:7`  | 
|  Europe \(London\)  |  `arn:aws:lambda:eu-west-2:282860088358:layer:AWS-AppConfig-Extension-Arm64:2`  | 
|  Asia Pacific \(Tokyo\)  |  `arn:aws:lambda:ap-northeast-1:980059726660:layer:AWS-AppConfig-Extension-Arm64:2`  | 
|  Asia Pacific \(Singapore\)  |  `arn:aws:lambda:ap-southeast-1:421114256042:layer:AWS-AppConfig-Extension-Arm64:3`  | 
|  Asia Pacific \(Sydney\)  |  `arn:aws:lambda:ap-southeast-2:080788657173:layer:AWS-AppConfig-Extension-Arm64:2`  | 
|  Asia Pacific \(Mumbai\)  |  `arn:aws:lambda:ap-south-1:554480029851:layer:AWS-AppConfig-Extension-Arm64:2`  | 

Date replaced by newer extension: 04/21/2022


**Version 2\.0\.45**  

| Region | ARN | 
| --- | --- | 
|  US East \(N\. Virginia\)  |  `arn:aws:lambda:us-east-1:027255383542:layer:AWS-AppConfig-Extension-Arm64:1`  | 
|  US East \(Ohio\)  |  `arn:aws:lambda:us-east-2:728743619870:layer:AWS-AppConfig-Extension-Arm64:1`  | 
|  US West \(Oregon\)  |  `arn:aws:lambda:us-west-2:359756378197:layer:AWS-AppConfig-Extension-Arm64:2`  | 
|  Europe \(Frankfurt\)  |  `arn:aws:lambda:eu-central-1:066940009817:layer:AWS-AppConfig-Extension-Arm64:1`  | 
|  Europe \(Ireland\)  |  `arn:aws:lambda:eu-west-1:434848589818:layer:AWS-AppConfig-Extension-Arm64:6`  | 
|  Europe \(London\)  |  `arn:aws:lambda:eu-west-2:282860088358:layer:AWS-AppConfig-Extension-Arm64:1`  | 
|  Asia Pacific \(Tokyo\)  |  `arn:aws:lambda:ap-northeast-1:980059726660:layer:AWS-AppConfig-Extension-Arm64:1`  | 
|  Asia Pacific \(Singapore\)  |  `arn:aws:lambda:ap-southeast-1:421114256042:layer:AWS-AppConfig-Extension-Arm64:2`  | 
|  Asia Pacific \(Sydney\)  |  `arn:aws:lambda:ap-southeast-2:080788657173:layer:AWS-AppConfig-Extension-Arm64:1`  | 
|  Asia Pacific \(Mumbai\)  |  `arn:aws:lambda:ap-south-1:554480029851:layer:AWS-AppConfig-Extension-Arm64:1`  | 