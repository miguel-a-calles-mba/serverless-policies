# ServerlessFramework

Use the `ServerlessBase.json` file to add the appropriate IAM permissions when you deploy an empty the Serverless Framework.

## Populate your stack information

Update the policy by replacing the placeholders with your stack information.

- Replace `{{region}}` with the desired region, e.g., `us-east-1`.
- Replace `{{accountId}}` with your AWS accound ID, e.g. `123456789012`.
- Replace `{{serviceName}}` with your service name, e.g. `myServiceName`.
- Replace `{{stage}}` with the stage, e.g., `dev`.
- Replace `{{cfRoleName}}` with the CloudFormation role name.
- Replace `{{stackName}}` with either:
  - The automatically generated stack name `{{serviceName}}-{{stage}}`.
  - The `stackName` property value, e.g., `myStackName`.

## Using a CloudFormation role

We will need multiple IAM items to deploy the following `serverless.yml` configuration.

```yaml
service: myServiceName

provider:
  name: aws
  runtime: nodejs12.x
  stage: ${env:STAGE, opt:stage, 'dev'}
  region: us-east-1
#  stackName: myStackName
  cfnRole: arn:aws:iam::123456789012:role/cfRoleName
```

We will need the following IAM items to create the `cfRoleName` CloudFormation role:

- `cfRoleName` IAM policy based on the `CloudFormationDeploy.json` file.
- `cfRoleName` IAM role that uses the `cfRoleName` IAM policy.

We will need the following IAM items to deploy via the Serverless Framework:

- An IAM policy based on the SIDs from both the `ServerlessFrameworkDeploy.json` and `AssumeCloudFormationRole.json` files.
- (Optional) An IAM group that uses the IAM policy.
- An IAM user that users the IAM policy (or is part of the IAM group).

## Regular Serverless Framework deploys

We will need one IAM policy to deploy the `serverless.yml` configuration above without the `cfnRole` property.

We will use the SIDs from both the `ServerlessFrameworkDeploy.json` and `CloudFormationDeploy.json` file to create the IAM policy.

The IAM policy will look like:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ServerlessFrameworkDeploy1",
            ...
        },
        {
            "Sid": "ServerlessFrameworkDeploy2",
            ...
        },
        {
            "Sid": "CloudFormationDeploy",
            ...
        }
    ]
}
```
