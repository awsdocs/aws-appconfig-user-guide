# Available versions of the AWS AppConfig Lambda extension<a name="appconfig-integration-lambda-extensions-versions"></a>

This topic lists AWS AppConfig Lambda extension versions according to processor\. The AWS AppConfig Lambda extension supports Lambda functions developed for the x86\-64 and ARM64 \(Graviton2\) platforms\. Each table includes the Amazon Resource Name \(ARN\) to use in each AWS Region\.

**Topics**
+ [x86\-64 platform](#appconfig-integration-lambda-extensions-enabling-x86-64)
+ [ARM64 platform](#appconfig-integration-lambda-extensions-enabling-ARM64)
+ [Older extension versions](#appconfig-integration-lambda-extensions-enabling-older-versions)

## x86\-64 platform<a name="appconfig-integration-lambda-extensions-enabling-x86-64"></a>

When you add the extension as a layer to your Lambda, you must specify an ARN\. Choose an ARN from the following table that corresponds with the AWS Region where you created the Lambda\. These ARNs are for Lambda functions developed for the x86\-64 platform\.


**Version 2\.0\.58**  

| Region | ARN | 
| --- | --- | 
|  US East \(N\. Virginia\)  |  `arn:aws:lambda:us-east-1:027255383542:layer:AWS-AppConfig-Extension:69`  | 
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
| China \(Bejing\) |  `arn:aws-cn:lambda:cn-north-1:615057806174:layer:AWS-AppConfig-Extension:46`  | 
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

## ARM64 platform<a name="appconfig-integration-lambda-extensions-enabling-ARM64"></a>

When you add the extension as a layer to your Lambda, you must specify an ARN\. Choose an ARN from the following table that corresponds with the AWS Region where you created the Lambda\. These ARNs are for Lambda functions developed for the ARM64 platform\.


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

## Older extension versions<a name="appconfig-integration-lambda-extensions-enabling-older-versions"></a>

This section lists the ARNs and AWS Regions for older versions of the AWS AppConfig Lambda extension\. This list doesn't contain information for all previous versions of the AWS AppConfig Lambda extension, but it will be updated when new versions are released\.

### Older extension versions \(x86\-64 platform\)<a name="appconfig-integration-lambda-extensions-enabling-older-versions-x86-64"></a>

The following tables list ARNs and the AWS Regions for older versions of the AWS AppConfig Lambda extension developed for the x86\-64 platform\.

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
| China \(Bejing\) |  `arn:aws-cn:lambda:cn-north-1:615057806174:layer:AWS-AppConfig-Extension:45`  | 
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
| China \(Bejing\) |  `arn:aws-cn:lambda:cn-north-1:615057806174:layer:AWS-AppConfig-Extension:43`  | 
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