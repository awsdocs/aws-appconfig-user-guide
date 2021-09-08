# AWS AppConfig User Guide

-----
*****Copyright &copy; Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in 
     connection with any product or service that is not Amazon's, 
     in any manner that is likely to cause confusion among customers, 
     or in any manner that disparages or discredits Amazon. All other 
     trademarks not owned by Amazon are the property of their respective
     owners, who may or may not be affiliated with, connected to, or 
     sponsored by Amazon.

-----
## Contents
+ [What Is AWS AppConfig?](what-is-appconfig.md)
+ [Getting Started with AWS AppConfig](getting-started-with-appconfig.md)
   + [Install or upgrade AWS command line tools](getting-started-cli.md)
   + [Configuring permissions for AWS AppConfig](getting-started-with-appconfig-permissions.md)
   + [(Optional) Configuring permissions for rollback based on CloudWatch alarms](getting-started-with-appconfig-cloudwatch-alarms-permissions.md)
   + [Security in AWS AppConfig](appconfig-security.md)
+ [Working with AWS AppConfig](appconfig-working.md)
   + [Step 1: Creating an AWS AppConfig application](appconfig-creating-application.md)
   + [Step 2: Creating an environment](appconfig-creating-environment.md)
   + [Step 3: Creating a configuration and a configuration profile](appconfig-creating-configuration-and-profile.md)
      + [About the configuration profile IAM role](appconfig-creating-configuration-and-profile-iam-role.md)
      + [About configurations stored in Amazon S3](appconfig-creating-configuration-and-profile-S3-source.md)
      + [About validators](appconfig-creating-configuration-and-profile-validators.md)
   + [Step 4: Creating a deployment strategy](appconfig-creating-deployment-strategy.md)
   + [Step 5: Deploying a configuration](appconfig-deploying.md)
   + [Step 6: Receiving the configuration](appconfig-retrieving-the-configuration.md)
+ [Services that integrate with AWS AppConfig](appconfig-integration.md)
   + [AWS AppConfig integration with CodePipeline](appconfig-integration-codepipeline.md)
   + [AWS AppConfig integration with Lambda extensions](appconfig-integration-lambda-extensions.md)
      + [Using a container image to add the AWS AppConfig Lambda extension](appconfig-integration-lambda-extensions-container-image.md)
      + [Integrating with OpenAPI](appconfig-integration-lambda-extensions-OpenAPI.md)
+ [AWS AppConfig User Guide Document History](doc-history.md)
+ [AWS glossary](glossary.md)