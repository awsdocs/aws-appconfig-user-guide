# AWS AppConfig integration with CodePipeline<a name="appconfig-integration-codepipeline"></a>

AWS AppConfig is an integrated deploy action for AWS CodePipeline \(CodePipeline\)\. CodePipeline is a fully managed continuous delivery service that helps you automate your release pipelines for fast and reliable application and infrastructure updates\. CodePipeline automates the build, test, and deploy phases of your release process every time there is a code change, based on the release model you define\. For more information, see [What is AWS CodePipeline?](https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html)

The integration of AWS AppConfig with CodePipeline offers the following benefits:
+ Customers who use CodePipeline to manage orchestration now have a lightweight means of deploying configuration changes to their applications without having to deploy their entire code base\.
+ Customers who want to use AWS AppConfig to manage configuration deployments but are limited because AWS AppConfig does not support their current code or configuration store, now have additional options\. CodePipeline supports AWS CodeCommit, GitHub, and BitBucket \(to name a few\)\.

**Note**  
 AWS AppConfig integration with CodePipeline is only supported in AWS Regions where CodePipeline is [available](aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.