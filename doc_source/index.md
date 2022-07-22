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
+ [Working with AWS AppConfig](appconfig-working.md)
   + [Step 1: Creating an AWS AppConfig application](appconfig-creating-application.md)
   + [Step 2: Creating an environment](appconfig-creating-environment.md)
   + [Step 3: Creating configuration profiles and feature flags](appconfig-creating-configuration-and-profile.md)
   + [Step 4: Creating a deployment strategy](appconfig-creating-deployment-strategy.md)
   + [Step 5: Deploying a configuration](appconfig-deploying.md)
   + [Step 6: Retrieving the configuration](appconfig-retrieving-the-configuration.md)
+ [Working with AWS AppConfig extensions](working-with-appconfig-extensions.md)
   + [About AWS AppConfig extensions](working-with-appconfig-extensions-about.md)
   + [Working with AWS-authored extensions](working-with-appconfig-extensions-about-predefined.md)
      + [Working with the AWS AppConfig deployment events to Amazon EventBridge extension](working-with-appconfig-extensions-about-predefined-notification-eventbridge.md)
      + [Working with the AWS AppConfig deployment events to Amazon SNS extension](working-with-appconfig-extensions-about-predefined-notification-sns.md)
      + [Working with the AWS AppConfig deployment events to Amazon SQS extension](working-with-appconfig-extensions-about-predefined-notification-sqs.md)
      + [Working with the Atlassian Jira extension for AWS AppConfig](working-with-appconfig-extensions-about-jira.md)
   + [Creating custom AWS AppConfig extensions](working-with-appconfig-extensions-creating-custom.md)
      + [Creating a Lambda function for a custom AWS AppConfig extension](working-with-appconfig-extensions-creating-custom-lambda.md)
      + [Configuring permissions for a custom AWS AppConfig extension](working-with-appconfig-extensions-creating-custom-permissions.md)
      + [Creating a custom AWS AppConfig extension](working-with-appconfig-extensions-creating-custom-extensions.md)
         + [Customizing AWS-authored notification extensions](working-with-appconfig-extensions-creating-custom-notification.md)
      + [Creating an extension association for a custom AWS AppConfig extension](working-with-appconfig-extensions-creating-custom-association.md)
      + [Performing an action that invokes a custom AWS AppConfig extension](working-with-appconfig-extensions-creating-custom-invoke.md)
+ [Services that integrate with AWS AppConfig](appconfig-integration.md)
   + [AWS AppConfig integration with Lambda extensions](appconfig-integration-lambda-extensions.md)
      + [Available versions of the AWS AppConfig Lambda extension](appconfig-integration-lambda-extensions-versions.md)
      + [Using a container image to add the AWS AppConfig Lambda extension](appconfig-integration-lambda-extensions-container-image.md)
      + [Integrating with OpenAPI](appconfig-integration-lambda-extensions-OpenAPI.md)
   + [AWS AppConfig integration with Jira](appconfig-integration-ref-jira.md)
   + [AWS AppConfig integration with CodePipeline](appconfig-integration-codepipeline.md)
+ [Security in AWS AppConfig](appconfig-security.md)
+ [Monitoring AWS AppConfig](appconfig-monitoring.md)
+ [AWS AppConfig User Guide Document History](doc-history.md)
+ [AWS glossary](glossary.md)