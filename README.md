# serverless-policies

AWS IAM policies you can use for your Serverless Framework projects.

## Changes you must make

Please update the placholders in the JSON files with specific values for your account and region.

- Replace `{{region}}` with your region (e.g., `us-east-1`).
- Replace `{{accountId}}` with your AWS account identifier.

You can choose to replace the placeholder with asterisks `*`, but the policies are no longer least privileged because it accepts all resources for the applicable ARN fields.

Note: In some cases, the entire ARN is an asterisk. This is sometimes done when the IAM policy statement does not accept an ARN. In other cases, the IAM policy visual editor recommended specifying all resources.

## Serverless base

The [ServerlessBaseDeploy.json](./policies/ServerlessBaseDeploy.json) are the minimum AWS IAM permissions you need to **deploy** an empty Serverless Framework stack.
The [ServerlessBaseRemove.json](./policies/ServerlessBaseRemove.json) are the minimum AWS IAM permissions you need to **remove** an empty Serverless Framework stack.
