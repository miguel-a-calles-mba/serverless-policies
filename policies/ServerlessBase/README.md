# ServerlessBase

Use the `ServerlessBase.json` file to add the appropriate IAM permissions when you deploy an empty the Serverless Framework.

## Populate your stack information

Update the policy by replacing the placeholders with your stack information.

- Replace `{{region}}` with the desired region, e.g., `us-east-1`
- Replace `{{accountId}}` with your AWS accound ID
- Replace `{{ServiceNameOrStackName}}` with the `service` or `stackName` property defined in the `serverless.yml` file

## Serverless base

Use all the SIDs to deploy the following `serverless.yml` configuration.

```yaml
service: service-name

provider:
  name: aws
  runtime: nodejs12.x
  stage: ${env:STAGE, opt:stage, 'dev'}
  region: us-west-1
#  stackName: stack-name
```
