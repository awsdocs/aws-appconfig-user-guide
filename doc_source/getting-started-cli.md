# Install or upgrade AWS command line tools<a name="getting-started-cli"></a>

This topic is for users who have *programmatic access* to use AWS AppConfig \(or any other AWS service\), and who want to run AWS CLI or AWS Tools for Windows PowerShell commands from their local machines\. 

**Note**  
Programmatic access and *console access* are different permissions that can be granted to a user account by an AWS account administrator\. A user can be granted one or both access types\. For information, see [Create non\-Admin IAM users and groups for Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-create-iam-user.html) in the *AWS Systems Manager User Guide*\.

For information about the AWS CLI, see the *[AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)*\. For information about the AWS Tools for Windows PowerShell, see the *[AWS Tools for Windows PowerShell User Guide](https://docs.aws.amazon.com/powershell/latest/userguide/)*\.

For information about all AWS AppConfig commands you can run using the AWS CLI, see [the AWS AppConfig section of the *AWS CLI Command Reference*](https://docs.aws.amazon.com/cli/latest/reference/appconfig/index.html)\. For information about all AWS AppConfig commands you can run using the AWS Tools for PowerShell, see [the AWS AppConfig section of the *AWS Tools for PowerShell Cmdlet Reference*](https://docs.aws.amazon.com/powershell/latest/reference/items/AWS_AppConfig_cmdlets.html)\.

------
#### [ AWS CLI ]

**To install or upgrade and then configure the AWS CLI**

1. Follow the instructions in [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide* to install or upgrade the AWS CLI on your local machine\.
**Tip**  
The AWS CLI is frequently updated with new functionality\. Upgrade \(reinstall\) the CLI periodically to ensure that you have access to all the latest functionality\.

1. To configure the AWS CLI, see [Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *AWS Command Line Interface User Guide*\.

   In this step, you specify credentials that an AWS administrator in your organization has given you, in the following format:

   ```
   AWS Access Key ID: AKIAIOSFODNN7EXAMPLE
   AWS Secret Access Key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   ```
**Important**  
When you configure the AWS CLI, you are prompted to specify an AWS Region\. Choose one of the supported Regions listed for Systems Manager in the [AWS General Reference](https://docs.aws.amazon.com/general/latest/gr/rande.html)\. If necessary, first verify with an administrator for your AWS account which Region you should choose\.

   For more information about access keys, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html) in the *IAM User Guide*

1. To verify the installation or upgrade, run the following command from the AWS CLI:

   ```
   aws appconfig help
   ```

   If successful, this command displays a list of available AWS AppConfig commands\.

------
#### [ AWS Tools for PowerShell ]

**To install or upgrade and then configure the AWS Tools for Windows PowerShell**

1. Follow the instructions in [Setting up the AWS Tools for Windows PowerShell or AWS Tools for PowerShell Core](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up.html) in the *AWS Tools for Windows PowerShell User Guide* to install or upgrade AWS Tools for PowerShell on your local machine\.
**Tip**  
AWS Tools for PowerShell is frequently updated with new functionality\. Upgrade \(reinstall\) the AWS Tools for PowerShell periodically to ensure that you have access to all the latest functionality\.

1. To configure AWS Tools for PowerShell, see [Using AWS Credentials](https://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html) in the *AWS Tools for Windows PowerShell User Guide*\.

   In this step, you specify credentials that an AWS administrator in your organization has given you, using the following command\.

   ```
   Set-AWSCredential `
   -AccessKey AKIAIOSFODNN7EXAMPLE `
   -SecretKey wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY `
   -StoreAs MyProfileName
   ```
**Important**  
When you configure AWS Tools for PowerShell, you can run `Set-DefaultAWSRegion` to specify an AWS Region\. Choose one of the supported Regions listed for AWS AppConfig in the [AWS General Reference](https://docs.aws.amazon.com/general/latest/gr/rande.html)\. If necessary, first verify with an administrator for your AWS account which Region you should choose\.

   For more information about access keys, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html) in the *IAM User Guide*\.

1. To verify the installation or upgrade, run the following command from AWS Tools for PowerShell\.

   ```
   Get-AWSCmdletName -Service AppConfig
   ```

   If successful, this command displays a list of available AWS AppConfig cmdlets\.

------