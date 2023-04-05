# Security in AWS AppConfig<a name="appconfig-security"></a>

Cloud security at AWS is the highest priority\. As an AWS customer, you benefit from a data center and network architecture that are built to meet the requirements of the most security\-sensitive organizations\.

Security is a shared responsibility between AWS and you\. The [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) describes this as security *of* the cloud and security *in* the cloud:
+ **Security of the cloud** – AWS is responsible for protecting the infrastructure that runs AWS services in the AWS Cloud\. AWS also provides you with services that you can use securely\. Third\-party auditors regularly test and verify the effectiveness of our security as part of the [AWS Compliance Programs](http://aws.amazon.com/compliance/programs/)\. To learn about the compliance programs that apply to AWS Systems Manager, see [AWS Services in Scope by Compliance Program](http://aws.amazon.com/compliance/services-in-scope/)\.
+ **Security in the cloud** – Your responsibility is determined by the AWS service that you use\. You are also responsible for other factors including the sensitivity of your data, your company’s requirements, and applicable laws and regulations\. 

AWS AppConfig is a capability of AWS Systems Manager\. To understand how to apply the shared responsibility model when using AWS AppConfig, see [Security in AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/security.html)\. That section describes how to configure Systems Manager to meet the security and compliance objectives for AWS AppConfig\.

## Data encryption at rest for AWS AppConfig<a name="appconfig-security-data-encryption"></a>

AWS AppConfig provides encryption by default to protect customer data at rest using AWS owned keys\.

**AWS owned keys** — AWS AppConfig uses these keys by default to automatically encrypt data deployed by the service and hosted in the AWS AppConfig data store\. You can't view, manage, or use AWS owned keys, or audit their use\. However, you don't have to take any action or change any programs to protect the keys that encrypt your data\. For more information, see [AWS owned keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-owned-cmk) in the *AWS Key Management Service Developer Guide*\.

While you can't disable this layer of encryption or select an alternate encryption type, you can add a customer managed key when you deploy your configuration data\.

**Customer managed keys** — AWS AppConfig supports the use of a symmetric customer managed key that you create, own, and manage to add a second layer of encryption over the existing AWS owned key\. Because you have full control of this layer of encryption, you can perform such tasks as:
+ Establishing and maintaining key policies and grants
+ Establishing and maintaining IAM policies
+ Enabling and disabling key policies
+ Rotating key cryptographic material
+ Adding tags
+ Creating key aliases
+ Scheduling keys for deletion

For more information, see [Customer managed key](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#customer-cmk) in the *AWS Key Management Service Developer Guide*\.

**AWS AppConfig supports customer managed keys**

AWS AppConfig offers support for customer managed key encryption for configuration data\. To encrypt configuration data, customers can add a `KmsKeyIdentifier` to their deployment\. AWS AppConfig generates the AWS KMS data key with this `KmsKeyIdentifier` to encrypt data on the `StartDeployment` API operation\.

**AWS AppConfig encryption access**

When creating a customer managed key, use the following key policy to ensure that the key can be used\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
        "Sid": "Allow use of the key",
        "Effect": "Allow",
        "Principal": {
            "AWS": "arn:aws:iam::account_ID:role/role_name"
        },
        "Action": [
            "kms:Decrypt",
            "kms:GenerateDataKey"
            ],
        "Resource": "*"
        }
 ]
```

If you are using a Secrets Manager secret or any other configuration data encrypted with a customer managed key, your `retrievalRoleArn` will need `kms:Decrypt` to decrypt and retrieve the data\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "kms:Decrypt",
      "Resource": "arn:aws:kms:Region:account_ID:configuration source/object"
    }
  ]
}
```

When calling the AWS AppConfig [StartDeployment](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_appconfigdata_StartDeployment.html) API operation, the identity calling `StartDeployment` needs the following IAM policy which can be assigned to a user, group, or role:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "kms:GenerateDataKey*"
      ],
      "Resource": "arn:aws:kms:Region:account_ID:key_ID"
    }
  ]
}
```

When calling the AWS AppConfig [GetLatestConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_appconfigdata_GetLatestConfiguration.html) API operation, the identity calling `GetLatestConfiguration` needs the following policy which can be assigned to a user, group, or role:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "kms:Decrypt,
            "Resource": "arn:aws:kms:Region:account_ID:key_ID"
        }
    ]
}
```

**Encryption context**

An [encryption context](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#encrypt_context) is an optional set of key\-value pairs that contain additional contextual information about the data\.

AWS KMS uses the encryption context as [additional authenticated data](https://docs.aws.amazon.com/crypto/latest/userguide/cryptography-concepts.html#term-aad) to support [authenticated encryption](https://docs.aws.amazon.com/crypto/latest/userguide/cryptography-concepts.html#define-authenticated-encryption)\. When you include an encryption context in a request to encrypt data, AWS KMS binds the encryption context to the encrypted data\. To decrypt data, you include the same encryption context in the request\.

 **AWS AppConfig encryption context**: AWS AppConfig uses the same encryption context in all AWS KMS cryptographic operations for encrypted deployments, where the key is `aws:appconfig:deployment:arn` and the value is the Amazon Resource Name \(ARN\) for the deployment\.

```
"encryptionContext": {
    "aws:appconfig:deployment:arn": "arn:aws:appconfig:Region:account_ID:application/application_ID/environment/environment_ID/deployment/deployment_number" 
 }
```

**Monitoring your encryption keys for AWS**

When you use an AWS KMS customer managed keys with AWS AppConfig, you can use AWS CloudTrail or Amazon CloudWatch Logs to track requests that AWS AppConfig sends to AWS KMS\.

The following example is a CloudTrail event for `Decrypt` to monitor AWS KMS operations called by AWS AppConfig to access data encrypted by your customer managed key:

```
{
    "eventVersion": "1.08",
    "userIdentity": {
        "type": "AWSService",
        "invokedBy": "appconfig.amazonaws.com"
    },
    "eventTime": "2023-01-03T02:22:28z",
    "eventSource": "kms.amazonaws.com",
    "eventName": "Decrypt",
    "awsRegion": "Region",
    "sourceIPAddress": "172.12.34.56",
    "userAgent": "ExampleDesktop/1.0 (V1; OS)",
    "requestParameters": {
        "encryptionContext": {
            "aws:appconfig:deployment:arn": "arn:aws:appconfig:Region:account_ID:application/application_ID/environment/environment_ID/deployment/deployment_ID"
        },
        "keyId": "arn:aws:kms:Region:account_ID:key/key_ID",
        "encryptionAlgorithm": "SYMMETRIC_DEFAULT"
    },
    "responseElements": null,
    "requestID": "ff000af-00eb-00ce-0e00-ea000fb0fba0SAMPLE",
    "eventID": "ff000af-00eb-00ce-0e00-ea000fb0fba0SAMPLE",
    "readOnly": true,
    "resources": [
        {
            "accountId": "account_ID",
            "type": "AWS::KMS::Key",
            "ARN": "arn:aws:kms:Region:account_ID:key_ID"
        }
    ],
    "eventType": "AwsApiCall",
    "managementEvent": true,
    "eventCategory": "Management",
    "recipientAccountId": "account_ID",
    "sharedEventID": "dc129381-1d94-49bd-b522-f56a3482d088"
}
```

## Secrets Manager key rotation<a name="appconfig-security-secrets-manager-key-rotation"></a>

This section describes important security information about AWS AppConfig integration with Secrets Manager\. For information about creating an AWS AppConfig configuration profile for Secrets Manager, see [Step 3: Creating configuration profiles and feature flags](appconfig-creating-configuration-and-profile.md)\. For information about Secrets Manager, see [What is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) in the *AWS Secrets Manager User Guide*\.

### Setting up automatic rotation of Secrets Manager secrets deployed by AWS AppConfig<a name="appconfig-security-secrets-manager-key-rotation-setting-up"></a>

*Rotation* is the process of periodically updating a secret stored in Secrets Manager\. When you rotate a secret, you update the credentials in both the secret and the database or service\. You can configure automatic secrets rotation in Secrets Manager by using an AWS Lambda function to update the secret and the database\. For more information, see [Rotate AWS Secrets Manager secrets](https://docs.aws.amazon.com/secretsmanager/latest/userguide/rotating-secrets.html) in the *AWS Secrets Manager User Guide*\.

To enable key rotation of Secrets Manager secrets deployed by AWS AppConfig, update your rotation Lambda function and deploy the rotated secret\.

**Note**  
Deploy you AWS AppConfig configuration profile after your secret has been rotated and fully updated to the new version\. You can determine if the secret rotated because the status of `VersionStage` changes from `AWSPENDING` to `AWSCURRENT`\. Secret rotation completion occurs within the Secrets Manager Rotation Templates `finish_secret` function\.

Here is an example function that starts an AWS AppConfig deployment after a secret is rotated\.

```
import time
import boto3
client = boto3.client('appconfig')

def finish_secret(service_client, arn, new_version):
    """Finish the rotation by marking the pending secret as current
    This method finishes the secret rotation by staging the secret staged AWSPENDING with the AWSCURRENT stage.
    Args:
        service_client (client): The secrets manager service client
        arn (string): The secret ARN or other identifier
        new_version (string): The new version to be associated with the secret
    """
    # First describe the secret to get the current version
    metadata = service_client.describe_secret(SecretId=arn)
    current_version = None
    for version in metadata["VersionIdsToStages"]:
        if "AWSCURRENT" in metadata["VersionIdsToStages"][version]:
            if version == new_version:
                # The correct version is already marked as current, return
                logger.info("finishSecret: Version %s already marked as AWSCURRENT for %s" % (version, arn))
                return
            current_version = version
            break

    # Finalize by staging the secret version current
    service_client.update_secret_version_stage(SecretId=arn, VersionStage="AWSCURRENT", MoveToVersionId=new_version, RemoveFromVersionId=current_version)
    
    # Deploy rotated secret
    response = client.start_deployment(
            ApplicationId='TestApp',
            EnvironmentId='TestEnvironment',
            DeploymentStrategyId='TestStrategy',
            ConfigurationProfileId='ConfigurationProfileId',
            ConfigurationVersion=new_version,
            KmsKeyIdentifier=key,
            Description='Deploy secret rotated at ' + str(time.time())
        )
   
    logger.info("finishSecret: Successfully set AWSCURRENT stage to version %s for secret %s." % (new_version, arn))
```