# Working with AWS AppConfig extensions<a name="working-with-appconfig-extensions"></a>

An extension augments your ability to inject logic or behavior at different points during the AWS AppConfig workflow of creating or deploying a configuration\. For example, you can use extensions to perform the following types of tasks \(to name a few\):
+ Send a notification to an Amazon Simple Notification Service \(Amazon SNS\) topic when a configuration profile is deployed\.
+ Scrub the contents of a configuration profile for sensitive data before a deployment starts\.
+ Create or update an Atlassian Jira issue whenever a change is made to a feature flag\.
+ Merge content from a service or data source into your configuration data when you start a deployment\.
+ Backup a configuration to an Amazon Simple Storage Service \(Amazon S3\) bucket whenever a configuration is deployed\. 

You can associate these types of tasks with AWS AppConfig applications, environments, and configuration profiles\.

**Topics**
+ [About AWS AppConfig extensions](working-with-appconfig-extensions-about.md)
+ [Working with AWS\-authored extensions](working-with-appconfig-extensions-about-predefined.md)
+ [Creating custom AWS AppConfig extensions](working-with-appconfig-extensions-creating-custom.md)