# Using a container image to add the AWS AppConfig Lambda extension<a name="appconfig-integration-lambda-extensions-container-image"></a>

You can package your AWS AppConfig Lambda extension as a container image to upload it to your container registry hosted on Amazon Elastic Container Registry \(Amazon ECR\)\. 

**To add the AWS AppConfig Lambda extension as a Lambda container image**

1. Enter the AWS Region and the Amazon Resource Name \(ARN\) in the AWS Command Line Interface \(AWS CLI\) as shown below\. Replace the Region and ARN value with your Region and the matching ARN to retrieve a copy of the Lambda layer\.

   ```
   aws lambda get-layer-version-by-arn \
     --region us-east-1 \
     --arn arn:aws:lambda::layer:AWS-AppConfig-Extension:00
   ```

    Following is the response\.

   ```
   {
     "LayerVersionArn": "arn:aws:lambda::layer:AWS-AppConfig-Extension:00", 
     "Description": "AWS AppConfig extension: Use dynamic configurations deployed via AWS AppConfig for your AWS Lambda functions",
     "CreatedDate": "2021-04-01T02:37:55.339+0000",  
     "LayerArn": "arn:aws:lambda::layer:AWS-AppConfig-Extension", 
   
     "Content": {
       "CodeSize": 5228073, 
       "CodeSha256": "8otOgbLQbexpUm3rKlMhvcE6Q5TvwcLCKrc4Oe+vmMY=", 
       "Location" : "S3-Bucket-Location-URL"
     },
   
     "Version": 30,
     "CompatibleRuntimes": [
       "python3.8",
       "python3.7",
       "nodejs12.x",
       "nodejs10.x",
       "ruby2.7"
     ],
   }
   ```

1. In the above response, the value returned for `Location` is the URL of the Amazon Simple Storage Service \(Amazon S3\) bucket that contains the Lambda extension\. Paste the URL into your web browser to download the Lambda extension \.zip file\. 
**Note**  
The Amazon S3 bucket URL is available for only 10 minutes\.

   \(Optional\) Alternatively, you can also use the following `curl` command to download the Lambda extension\.

   ```
   curl -o extension.zip "S3-Bucket-Location-URL" 
   ```

   \(Optional\) Alternatively, you can combine **Step 1** and **Step 2** to retrieve the ARN and download the \.zip extension file all at once\. 

   ```
   aws lambda get-layer-version-by-arn \
     --arn arn:aws:lambda::layer:AWS-AppConfig-Extension:00 \
     | jq -r '.Content.Location' \
     | xargs curl -o extension.zip
   ```

1. Add the following lines in your `Dockerfile` to add the extension to your container image\.

   ```
   COPY extension.zip extension.zip
   RUN yum install -y unzip \
     && unzip extension.zip /opt \
     && rm -f extension.zip
   ```

1. Ensure that the Lambda function execution role has the [appconfig:GetConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetConfiguration.html) permission set\. 

## Example<a name="appconfig-lambda-extension-container-example"></a>

This section includes an example for enabling the AWS AppConfig Lambda extension on a container image\-based Python Lambda function\.

1. Create a `Dockerfile` that is similar to the following\.

   ```
   FROM public.ecr.aws/lambda/python:3.8 AS builder
   COPY extension.zip extension.zip
   RUN yum install -y unzip \
     && unzip extension.zip -d /opt \
     && rm -f extension.zip
   
   FROM public.ecr.aws/lambda/python:3.8
   COPY --from=builder /opt /opt
   COPY index.py ${LAMBDA_TASK_ROOT}
   CMD [ "index.handler" ]
   ```

1. Download the extension layer to the same directory as the `Dockerfile`\. 

   ```
   aws lambda get-layer-version-by-arn \
     --arn arn:aws:lambda::layer:AWS-AppConfig-Extension:00 \
     | jq -r '.Content.Location' \
     | xargs curl -o extension.zip
   ```

1. Create a Python file named `index.py` in the same directory as the `Dockerfile`\. 

   ```
   import urllib.request
   
   def handler(event, context):
       return {
          # replace parameters here with your application, environment, and configuration names
          'configuration': get_configuration('myApp', 'myEnv', 'myConfig'),
       }
   
   def get_configuration(app, env, config):
       url = f'http://localhost:2772/applications/{app}/environments/{env}/configurations/{config}'
       return urllib.request.urlopen(url).read()
   ```

1. Run the following steps to build the `docker` image and upload it to Amazon ECR\.

   ```
   // set environment variables
   export ACCOUNT_ID = <YOUR_ACCOUNT_ID>
   export REGION = <AWS_REGION>
   
   // create an ECR repository
   aws ecr create-repository --repository-name test-repository
   
   // build the docker image
   docker build -t test-image .
   
   // sign in to AWS
   aws ecr get-login-password \
     | docker login \
     --username AWS \
     --password-stdin "$ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com"
   
   // tag the image 
   docker tag test-image:latest "$ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/test-repository:latest"
   
   // push the image to ECR 
   docker push "$ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/test-repository:latest"
   ```

1. Use the Amazon ECR image that you created above to create the Lambda function\. For more information about a Lambda function as a container, see [Create a Lambda function defined as a container image](https://docs.aws.amazon.com/lambda/latest/dg/getting-started-create-function.html#gettingstarted-images-function)\. 

1. Ensure that the Lambda function execution role has the [appconfig:GetConfiguration](https://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetConfiguration.html) permission set\. 