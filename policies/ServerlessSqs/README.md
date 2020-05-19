# ServerlessSqs

Use the `ServerlessSqsDeploy.json` file to add the appropriate IAM permissions when you deploy an SQS queue using the Serverless Framework.

## Populate your stack information

Update the policy by replacing the placeholders with your stack information.

- Replace `{{region}}` with the desired region, e.g., `us-east-1`
- Replace `{{accountId}}` with your AWS accound ID
- Replace `{{queueName}}` with the queue name defined in the `serverless.yml` file

## Basic DynamoDB table

Use the `ServerlessSqsBaseDeploy` SID to create a queue with the following `serverless.yml` configuration.

```yaml
resources:
  Resources:
    MySqsQueue:
      Type: AWS::SQS::Queue
      Properties:
        QueueName: queueName
```

Add the `ServerlessSqsPropertiesDeploy` SID to create a queue with the following `serverless.yml` configuration.

```yaml
resources:
  Resources:
    MySqsQueue:
      Type: AWS::SQS::Queue
      Properties:
        QueueName: queueName
        DelaySeconds: 0
        MaximumMessageSize: 262144
        MessageRetentionPeriod: 345600
        ReceiveMessageWaitTimeSeconds: 0
        VisibilityTimeout: 30
```

Add the `ServerlessSqsTagsDeploy` SID to create a queue with the following `serverless.yml` configuration.

```yaml
resources:
  Resources:
    MySqsQueue:
      Type: AWS::SQS::Queue
      Properties:
        QueueName: queueName
        Tags:
          - Key: keyname
            Value: keyvalue
```

Add the `ServerlessSqsPolicyDeploy` SID to create a queue with the following `serverless.yml` configuration.

```yaml
resources:
  Resources:
    MySqsQueue:
      Type: AWS::SQS::Queue
      Properties:
        QueueName: queueName
    M7SqsQueuePolicy:
      Type: AWS::SQS::QueuePolicy
      Properties: 
        PolicyDocument:
          Version: "2012-10-17"
          Statement:
            - Effect: Allow
              Principal: "*"
              Action: SQS:SendMessage
              Resource: !Ref MySqsQueue
```
