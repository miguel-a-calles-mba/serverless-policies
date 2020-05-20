# Lambda

Use the `LambdaDeploy.json` file to add the appropriate IAM permissions when you deploy a Lambda function using the Serverless Framework.

## Populate your stack information

Update the policy by replacing the placeholders with your stack information.

- Replace `{{region}}` with the desired region, e.g., `us-east-1`
- Replace `{{accountId}}` with your AWS accound ID
- Replace `{{stage}}` with the stage, e.g., `dev`
- Replace `{{stackName}}` with the `service` defined in the `serverless.yml` file
- Replace `{{functionName}}` with the function name defined in the `serverless.yml` file

## Basic Lambda Function

Use the `LambdaBase` SID and one `Lambda{{functionName}}` SID per function to create a Lambda function with the following `serverless.yml` configuration.

```yaml
functions:
  functionName:
    handler: functionName.handler
```
