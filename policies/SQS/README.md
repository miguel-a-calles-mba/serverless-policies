# SQS

Use the `SqsDeploy.json` file to add the appropriate IAM permissions when you deploy an SQS queue using the Serverless Framework.

## Populate your stack information

Update the policy by replacing the placeholders with your stack information.

- Replace `{{region}}` with the desired region, e.g., `us-east-1`
- Replace `{{accountId}}` with your AWS accound ID
- Replace `{{queueName}}` with the queue name defined in the `serverless.yml` file

## Basic SQS Queue

Use the `SqsQueueBaseDeploy` SID to create a queue with the following `serverless.yml` configuration.

```yaml
resources:
  Resources:
    MySqsQueue:
      Type: AWS::SQS::Queue
      Properties:
        QueueName: queueName
```

## SQS Queue with Additional Properties

Add the `SqsQueuePropertiesDeploy` SID to create a queue with the following `serverless.yml` configuration.

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

Add the `SqsQueueTagsDeploy` SID to create a queue with the following `serverless.yml` configuration.

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

## SQS Queue with a Queue Policy

Add the `SqsQueuePolicyDeploy` SID to create a queue with the following `serverless.yml` configuration.

```yaml
resources:
  Resources:
    MySqsQueue:
      Type: AWS::SQS::Queue
      Properties:
        QueueName: queueName
    MySqsQueuePolicy:
      Type: AWS::SQS::QueuePolicy
      Properties: 
        PolicyDocument:
          Version: "2012-10-17"
          Statement:
            - Effect: Allow
              Principal: "*"
              Action: SQS:SendMessage
              Resource: !Ref MySqsQueue
        Queues: 
          - !Ref MySqsQueue
```
