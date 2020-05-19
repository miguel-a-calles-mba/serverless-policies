# serverless-policies

AWS IAM policies you can use for your Serverless Framework projects.

## Segmented policy files

The policies JSON are segmented to allow you to choose which IAM policies you want to use.

At a minimum, you need all the SIDs in the ServerlessBase policy to deploy a `serverless.yml` file.

You add more SIDs depending on what else you are adding to your deployment.

## Example

Let's suppose you want to deploy a basic DynamoDB table using your `serverless.yml` file.

```yaml
service: service-name

provider:
  name: aws
  runtime: nodejs12.x
  stage: ${env:STAGE, opt:stage, 'dev'}
  region: us-west-1

resources:
  Resources:
    Table:
      Type: AWS::DynamoDB::Table
      Properties:
        TableName: ${self:service}-${self:provider.stage}-table
        AttributeDefinitions:
          - AttributeName: UniqueId
            AttributeType: S
          - AttributeName: SortItem
            AttributeType: S
        KeySchema:
          - AttributeName: UniqueId
            KeyType: HASH
          - AttributeName: SortItem
            KeyType: RANGE
        ProvisionedThroughput:
          ReadCapacityUnits: 1
          WriteCapacityUnits: 1
```

We use all the SIDs from the ServerlessBase policy and the base SID in the DynamoDbDeploy policy.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ServerlessBaseDeploy1",
            "Effect": "Allow",
            "Action": [
                "s3:PutEncryptionConfiguration",
                "s3:PutBucketTagging",
                "s3:PutBucketPolicy",
                "s3:CreateBucket",
                "s3:ListBucket",
                "cloudformation:ValidateTemplate"
            ],
            "Resource": "*"
        },
        {
            "Sid": "ServerlessBaseDeploy2",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "cloudformation:DescribeStackEvents",
                "cloudformation:CreateStack",
                "cloudformation:TagResource",
                "cloudformation:UpdateStack",
                "cloudformation:DescribeStackResource",
                "cloudformation:DescribeStacks",
                "cloudformation:ListStackResources"
            ],
            "Resource": [
                "arn:aws:cloudformation:{{region}}:{{accountId}}:stack/{{ServiceNameOrStackName}}*/*",
                "arn:aws:cloudformation:{{region}}:{{accountId}}:stackset/{{ServiceNameOrStackName}}*:*",
                "arn:aws:s3:::{{ServiceNameOrStackName}}*/*"
            ]
        },
        {
            "Sid": "DynamoDbBaseDeploy",
            "Effect": "Allow",
            "Action": [
                "dynamodb:CreateTable",
                "dynamodb:DescribeTable",
                "dynamodb:DescribeTimeToLive"
            ],
            "Resource": "arn:aws:dynamodb:{{region}}:{{accountId}}:table/{{tableName}}"
        }
    ]
}
```

We replace the placeholders with the information from our `serverless.yml` file.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ServerlessBaseDeploy1",
            "Effect": "Allow",
            "Action": [
                "s3:PutEncryptionConfiguration",
                "s3:PutBucketTagging",
                "s3:PutBucketPolicy",
                "s3:CreateBucket",
                "s3:ListBucket",
                "cloudformation:ValidateTemplate"
            ],
            "Resource": "*"
        },
        {
            "Sid": "ServerlessBaseDeploy2",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "cloudformation:DescribeStackEvents",
                "cloudformation:CreateStack",
                "cloudformation:TagResource",
                "cloudformation:UpdateStack",
                "cloudformation:DescribeStackResource",
                "cloudformation:DescribeStacks",
                "cloudformation:ListStackResources"
            ],
            "Resource": [
                "arn:aws:cloudformation:us-west-1:123456789012:stack/service-name*/*",
                "arn:aws:cloudformation:us-west-1:123456789012:stackset/service-name*:*",
                "arn:aws:s3:::service-name*/*"
            ]
        },
        {
            "Sid": "DynamoDbBaseDeploy",
            "Effect": "Allow",
            "Action": [
                "dynamodb:CreateTable",
                "dynamodb:DescribeTable",
                "dynamodb:DescribeTimeToLive"
            ],
            "Resource": "arn:aws:dynamodb:us-west-1:123456789012:table/service-name-dev-table"
        }
    ]
}
```
