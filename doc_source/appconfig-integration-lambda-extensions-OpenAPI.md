# Integrating with OpenAPI<a name="appconfig-integration-lambda-extensions-OpenAPI"></a>

You can use the following YAML specification for OpenAPI to create an SDK using a tool like [OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator)\. You can update this specification to include hardcoded values for Application, Environment, or Configuration\. You can also add additional paths \(if you have multiple configuration types\) and include configuration schemas to generate configuration\-specific typed models for your SDK clients\. For more information about OpenAPI \(which is also known as Swagger\), see the [OpenAPI specification](https://swagger.io/specification/)\.

```
openapi: 3.0.0
info:
  version: 1.0.0
  title: AppConfig Lambda extension API
  description: An API model for AppConfig Lambda extension. 
servers:
  - url: https://localhost:{port}/
    variables:
      port:
        default:
          '2772'
paths:
  /applications/{Application}/environments/{Environment}/configurations/{Configuration}:
    get:
      operationId: getConfiguration
      tags:
        - configuration
      parameters:
        - in: path
          name: Application
          description: The application for the configuration to get. Specify either the application name or the application ID.
          required: true
          schema:
            type: string
        - in: path
          name: Environment
          description: The environment for the configuration to get. Specify either the environment name or the environment ID.
          required: true
          schema:
            type: string
        - in: path
          name: Configuration
          description: The configuration to get. Specify either the configuration name or the configuration ID.
          required: true
          schema:
            type: string
      responses:
        200:
          headers:
            ConfigurationVersion:
              schema:
                type: string
          content:
            application/octet-stream:
              schema:
                type: string
                format: binary
          description: successful config retrieval
        400:
          description: BadRequestException
          content:
            application/text:
              schema:
                $ref: '#/components/schemas/Error'
        404:
          description: ResourceNotFoundException
          content:
            application/text:
              schema:
                $ref: '#/components/schemas/Error'
        500:
          description: InternalServerException
          content:
            application/text:
              schema:
                $ref: '#/components/schemas/Error'
        502:
          description: BadGatewayException
          content:
            application/text:
              schema:
                $ref: '#/components/schemas/Error'
        504:
          description: GatewayTimeoutException
          content:
            application/text:
              schema:
                $ref: '#/components/schemas/Error'

components:
  schemas:
    Error:
      type: string
      description: The response error
```