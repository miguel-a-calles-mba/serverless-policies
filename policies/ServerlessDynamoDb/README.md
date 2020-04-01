# ServerlessDynamoDb

```
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
