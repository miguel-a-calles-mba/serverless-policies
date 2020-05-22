# ServerlessFramework

Use the `ServerlessBase.json` file to add the appropriate IAM permissions when you deploy an empty the Serverless Framework.

## Populate your stack information

Update the policy by replacing the placeholders with your stack information.

- Replace `{{region}}` with the desired region, e.g., `us-east-1`.
- Replace `{{accountId}}` with your AWS accound ID, e.g. `123456789012`.
- Replace `{{serviceName}}` with your service name, e.g. `myServiceName`.
- Replace `{{stage}}` with the stage, e.g., `dev`.
- Replace `{{stackName}}` with either:
  - The automatically generated stack name `{{serviceName}}-{{stage}}`.
  - The `stackName` property value, e.g., `myStackName`.

## Using a CloudFormation role

We will need two IAM polices to deploy the following `serverless.yml` configuration.

```yaml
service: myServiceName

provider:
  name: aws
  runtime: nodejs12.x
  stage: ${env:STAGE, opt:stage, 'dev'}
  region: us-east-1
#  stackName: myStackName
  cfnRole: arn:aws:iam::123456789012:role/my-cfn-role
```

We will need an IAM policy that the `my-cfn-role` uses. Use the `CloudFormationDeploy.json` policy.

We will need an IAM policy for the IAM user that performs the deploy. Use the `ServerlessFrameworkDeploy.json` file.

To illustrate the IAM relationships:

- The `my-cfn-role` uses the `my-cfn-policy` created with the `CloudFormationDeploy.json` file.
- The IAM users uses the `my-serverless-policy` policy (or is part of an IAM group that uses that policy). The `my-serverless-policy` was created with the `ServerlessFrameworkDeploy.json` file.

## Regular Serverless Framework deploys

We will need one IAM policy to deploy the `serverless.yml` configuration above without the `cfnRole` property.

We will use the SIDs from both the `ServerlessFrameworkDeploy.json` and `CloudFormationDeploy.json` file to create the IAM policy.
