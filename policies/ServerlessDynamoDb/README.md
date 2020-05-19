# ServerlessDynamoDb

Use the `ServerlessDynamoDBDeploy.json` file to add the appropriate IAM permissions when you deploy a DynamoDB table using the Serverless Framework.

## Populate your stack information

Update the policy by replacing the placeholders with your stack information.

- Replace `{{region}}` with the desired region, e.g., `us-east-1`
- Replace `{{accountId}}` with your AWS accound ID
- Replace `{{tableName}}` with the table name defined in the `serverless.yml` file

## Basic DynamoDB table

Use the `DynamoDbBaseDeploy` SID to create a table with the following `serverless.yml` configuration.

```yaml
resources:
  Resources:
    Table:
      Type: AWS::DynamoDB::Table
      Properties:
        TableName: ${self:service}-${self:provider.stage}
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
