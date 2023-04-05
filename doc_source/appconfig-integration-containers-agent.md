# AWS AppConfig integration with Amazon ECS and Amazon EKS<a name="appconfig-integration-containers-agent"></a>

You can integrate AWS AppConfig with Amazon Elastic Container Service \(Amazon ECS\) and Amazon Elastic Kubernetes Service \(Amazon EKS\) by using the AWS AppConfig agent\. The agent functions as a sidecar container running alongside your Amazon ECS and Amazon EKS container applications\. The agent enhances containerized application processing and management in the following ways:
+ The agent calls AWS AppConfig on your behalf by using an AWS Identity and Access Management \(IAM\) role and managing a local cache of configuration data\. By pulling configuration data from the local cache, your application requires fewer code updates to manage configuration data, retrieves configuration data in milliseconds, and isn't affected by network issues that can disrupt calls for such data\.\*
+ The agent offers a native experience for retrieving and resolving AWS AppConfig feature flags\.
+ Out of the box, the agent provides best practices for caching strategies, polling intervals, and local configuration data availability while tracking the configuration tokens needed for subsequent service calls\.
+ While running in the background, the agent periodically polls the AWS AppConfig data plane for configuration data updates\. Your containerized application can retrieve the data by connecting to localhost on port 2772 \(a customizable default port value\) and calling HTTP GET to retrieve the data\.
+ The AWS AppConfig agent updates configuration data in your containers without having to restart or recycle those containers\.

\*The AWS AppConfig agent caches data the first time the service retrieves your configuration data\. For this reason, the first call to retrieve data is slower than subsequent calls\.

**Topics**
+ [Before you begin](#w74aac17b9c11)
+ [Starting the AWS AppConfig agent for Amazon ECS integration](#appconfig-integration-containers-agent-starting-ecs)
+ [Starting the AWS AppConfig agent for Amazon EKS integration](#appconfig-integration-containers-agent-starting-eks)
+ [Using environment variables to configure the AWS AppConfig agent for Amazon ECS and Amazon EKS](#appconfig-integration-containers-agent-configuring)
+ [Retrieving configuration data](#appconfig-integration-containers-agent-retrieving-data)

## Before you begin<a name="w74aac17b9c11"></a>

To integrate AWS AppConfig with your container applications, you must complete Steps 1 through 5 in [Working with AWS AppConfig](appconfig-working.md)\. These steps create important AWS AppConfig artifacts and resources\.

To retrieve configuration data hosted by AWS AppConfig, your container applications must be configured with access to the AWS AppConfig data plane\. To give your applications access, update the IAM permissions policy that is used by your container service IAM role\. Specifically, you must add the `appconfig:StartConfigurationSession` and `appconfig:GetLatestConfiguration` actions to the policy\. Container service IAM roles include the following:
+ The Amazon ECS task role
+ The Amazon EKS node role
+ The AWS Fargate \(Fargate\) pod execution role \(if your Amazon EKS containers use Fargate for compute processing\)

For more information about adding permissions to a policy, see [Adding and removing IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\.

## Starting the AWS AppConfig agent for Amazon ECS integration<a name="appconfig-integration-containers-agent-starting-ecs"></a>

The AWS AppConfig agent sidecar container is automatically available in your Amazon ECS environment\. To use the AWS AppConfig agent sidecar container, you must start it\.

**To start the AWS AppConfig agent \(console\)**

1. Open the Amazon ECS classic console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the navigation pane, choose **Task definitions**\.

1. Choose the task definition for your application, and then choose the latest revision\.

1. Choose **Create new revision** from the **Create new revision** dropdown list\.

1. Choose **Add more containers**\.

1. Enter a name for the AWS AppConfig agent container\.

1. For **Image URI**, enter: **public\.ecr\.aws/aws\-appconfig/aws\-appconfig\-agent:2\.x**

1. For **Essential container**, choose **Yes**\.

1. In the **Port mappings** section, choose **Add**\.

1. For **Container port**, enter **2772**\.
**Note**  
The AWS AppConfig agent runs on port 2772, by default\. You can specify a different port\.

1. Choose **Create**\. Amazon ECS creates a new container revision and displays the details\.

1. In the navigation pane, choose **Clusters**, and then choose your application cluster in the list\.

1. In the **Services** section, choose the service for your application\.

1. Choose **Edit service**\.

1. For **Revision**, choose the latest revision from the dropdown list\.

1. Choose **Update**\. Amazon ECS deploys the latest task definition\.

1. After the deployment finishes, you can verify that the AWS AppConfig agent is running on the **Configuration and tasks** tab\. In the **Tasks** section, choose the running task\.

1. In the **Containers** section, verify that the AWS AppConfig agent container is listed\.

1. To verify that the AWS AppConfig agent started, choose the **Logs** tab\. Locate a statement like the following for the AWS AppConfig agent container: \[appconfig agent\] 1970/01/01 00:00:00 INFO serving on localhost:2772

**Note**  
You can adjust the default behavior of the AWS AppConfig agent by entering or changing environment variables\. For information about the available environment variables, see [Using environment variables to configure the AWS AppConfig agent for Amazon ECS and Amazon EKS](#appconfig-integration-containers-agent-configuring)\. For information about how to change environment variables in Amazon ECS, see [Passing environment variables to a container](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/taskdef-envfiles.html) in the *Amazon Elastic Container Service Developer Guide*\.

## Starting the AWS AppConfig agent for Amazon EKS integration<a name="appconfig-integration-containers-agent-starting-eks"></a>

The AWS AppConfig agent sidecar container is automatically available in your Amazon EKS environment\. To use the AWS AppConfig agent sidecar container, you must start it\. The following procedure describes how to use the Amazon EKS `kubectl` command line tool to make changes in the `kubeconfig` file for your container application\. For more information about creating or editing a `kubeconfig` file, see [Creating or updating a kubeconfig file for an Amazon EKS cluster](https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html) in the **Amazon EKS User Guide**\.

**To start the AWS AppConfig agent \(kubectl command line tool\)**

1. Open your `kubeconfig` file and verify that your Amazon EKS application is running as a single\-container deployment\. The contents of the file should look similar to the following\.

   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: my-app
     namespace: my-namespace
     labels:
       app: my-application-label
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: my-application-label
     template:
       metadata:
         labels:
           app: my-application-label
       spec:
         containers:
         - name: my-app
           image: my-repo/my-image
           imagePullPolicy: IfNotPresent
   ```

1. Add the AWS AppConfig agent container definition details to your YAML deployment file\.

   ```
   - name: appconfig-agent
           image: public.ecr.aws/aws-appconfig/aws-appconfig-agent:2.x
           ports:
           - name: http
             containerPort: 2772
             protocol: TCP
           env:
           - name: SERVICE_REGION
             value: region
           imagePullPoliy: IfNotPresent
   ```
**Note**  
Note the following information\.  
The AWS AppConfig agent runs on port 2772, by default\. You can specify a different port\.
You can adjust the default behavior of the AWS AppConfig agent by entering environment variables\. For more information, see [Using environment variables to configure the AWS AppConfig agent for Amazon ECS and Amazon EKS](#appconfig-integration-containers-agent-configuring)\.
For *SERVICE\_REGION*, specify the AWS Region code \(for example, `us-west-1`\) where the AWS AppConfig agent retrieves configuration data\.

1. Run the following command in the `kubectl` tool to apply the changes to your cluster\.

   ```
   kubectl apply -f my-deployment.yml
   ```

1. After the deployment finishes, verify that the AWS AppConfig agent is running\. Use the following command to view the application pod log file\.

   ```
   kubectl logs -n my-namespace -c appconfig-agent my-pod
   ```

   Locate a statement like the following for the AWS AppConfig agent container: \[appconfig agent\] 1970/01/01 00:00:00 INFO serving on localhost:2772

**Note**  
You can adjust the default behavior of the AWS AppConfig agent by entering or changing environment variables\. For information about the available environment variables, see [Using environment variables to configure the AWS AppConfig agent for Amazon ECS and Amazon EKS](#appconfig-integration-containers-agent-configuring)\.

## Using environment variables to configure the AWS AppConfig agent for Amazon ECS and Amazon EKS<a name="appconfig-integration-containers-agent-configuring"></a>

You can configure the AWS AppConfig agent by changing the following environment variables for your agent container\.


****  

| Environment variable | Details | Default value | 
| --- | --- | --- | 
|  `ACCESS_TOKEN`  |  This environment variable defines a token that must be provided when requesting configuration data from the agent HTTP server\. The value of the token must be set in the HTTP request authorization header with an authorization type of `Bearer`\. Here is an example\. <pre>GET /applications/my_app/...<br />                  Host: localhost:2772<br />                  Authorization: Bearer <token value></pre>  | None | 
|  `HTTP_PORT`  |  This environment variable specifies the port on which the HTTP server for the agent runs\.  | 2772 | 
|  `LOG_LEVEL`  |  This environment variable specifies the level of detail that the agent logs\. Each level includes the current level and all higher levels\. The variables are case sensitive\. From most to least detailed, the log levels are: `debug`, `info`, `warn`, `error`, and `none`\. `Debug` includes detailed information, including timing information, about the agent\.  |  `info`  | 
|  `MAX_CONNECTIONS`  |  This environment variable configures the maximum number of connections that the agent uses to retrieve configurations from AWS AppConfig\.   | 3 | 
|  `POLL_INTERVAL`  |  This environment variable controls how often the agent polls AWS AppConfig for updated configuration data\. You can specify a number of seconds for the interval\. You can also specify a number with a time unit: s for seconds, m for minutes, and h for hours\. If a unit isn't specified, the agent defaults to seconds\. For example, 60, 60s, and 1m result in the same poll interval\.   | 45 seconds | 
|  `REQUEST_TIMEOUT`  |  This environment variable controls the amount of time the agent waits for a response from AWS AppConfig\. If the service does not respond, the request fails\. If the request is for the initial data retrieval, the agent returns an error to your application\. If the timeout occurs during a background check for updated data, the agent logs the error and tries again after a short delay\. You can specify the number of milliseconds for the timeout\. You can also specify a number with a time unit: ms for milliseconds and s for seconds\. If a unit isn't specified, the agent defaults to milliseconds\. As an example, 5000, 5000ms and 5s result in the same request timeout value\.  | 3000 milliseconds | 
|  `PREFETCH_LIST`  |  This environment variable specifies the configuration data the agent requests from AWS AppConfig as soon as it starts\.  | None | 
| PROXY\_HEADERS | This environment variable specifies headers that are required by the proxy referenced in the PROXY\_URL environment variable\. The value is a comma\-separated list of headers\. Each header uses the following form\. <pre>"header: value"</pre> | None | 
| PROXY\_URL | This environment variable specifies the proxy URL to use for connections from the agent to AWS services, including AWS AppConfig\. HTTPS and HTTP URLs are supported\. | None | 
| ROLE\_ARN | This environment variable specifies the Amazon Resource Name \(ARN\) of an IAM role\. The AWS AppConfig agent assumes this role to retrieve configuration data\. | None | 
| ROLE\_EXTERNAL\_ID | This environment variable specifies the external ID to use with the assumed role ARN\. | None | 
| ROLE\_SESSION\_NAME | This environment variable specifies the session name to be associated with the credentials for the assumed IAM role\. | None | 
| SERVICE\_REGION | This environment variable specifies an alternative AWS Region that AWS AppConfig Agent uses to call the AWS AppConfig service\. If left undefined, the agent attempts to determine the current Region\. If it can't, the agent fails to start\. | None | 

## Retrieving configuration data<a name="appconfig-integration-containers-agent-retrieving-data"></a>

You can retrieve configuration data from the AWS AppConfig agent by using an HTTP localhost call\. The following examples use `curl` with an HTTP client\. You can call the agent using any available HTTP client supported by your application language or available libraries\.

** To retrieve the full content of any deployed configuration**

```
$ curl "http://localhost:2772/applications/application_name/environments/environment_name/configurations/configuration_name"
```

**To retrieve a single flag and its attributes from an AWS AppConfig configuration of type `Feature Flag`**

```
$ curl "http://localhost:2772/applications/application_name/environments/environment_name/configurations/configuration_name?flag=flag_name"
```

**To access multiple flags and their attributes from an AWS AppConfig configuration of type `Feature Flag`**

```
$ curl "http://localhost:2772/applications/application_name/environments/environment_name/configurations/configuration_name?flag=flag_name_one&flag=flag_name_two"
```