# Enabling Long Polling in Amazon SQS Queues<a name="sqs-example-enable-long-polling"></a>

These AWS SDK for Go examples show you how to:
+ Enable long polling when you create an Amazon SQS queue
+ Enable long polling on an existing Amazon SQS queue
+ Enable long polling when a message is received

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/sqs) repository on GitHub\.

## Scenario<a name="sqs-long-polling-scenario"></a>

Long polling reduces the number of empty responses by allowing Amazon SQS to wait a specified time for a message to become available in the queue before sending a response\. Also, long polling eliminates false empty responses by querying all of the servers instead of a sampling of servers\. To enable long polling, you must specify a non\-zero wait time for received messages\. You can do this by setting the `ReceiveMessageWaitTimeSeconds` parameter of a queue or by setting the `WaitTimeSeconds` parameter on a message when it is received\.

The code uses these methods of the Amazon SQS client class:
+  [CreateQueue](https://docs.aws.amazon.com/sdk-for-go/api/service/sqs/#SQS.CreateQueue) 
+  [SetQueueAttributes](https://docs.aws.amazon.com/sdk-for-go/api/service/sqs/#SQS.SetQueueAttributes) 
+  [ReceiveMessage](https://docs.aws.amazon.com/sdk-for-go/api/service/sqs/#SQS.ReceiveMessage) 

## Prerequisites<a name="sqs-long-polling-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with Amazon SQS polling\. To learn more, see [Long Polling](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-long-polling.html) in the Amazon SQS Developer Guide\.

## Enable Long Polling When Creating a Queue<a name="sqs-example-create-queue-long-pollling"></a>

This example creates a queue with long polling enabled\. If the queue already exists, no error is returned\.

Create a new Go file named `CreateLPQueue.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "flag"
    "fmt"
    "strconv"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/sqs"
)
```

Get the queue name and wait time from the command line\. Ensure that the wait time is between 0 \(zero\) and 20 seconds\.

```
queue := flag.String("q", "", "The name of the queue")
waitTime := flag.Int("w", 10, "How long, in seconds, to wait for long polling")
flag.Parse()

if *queue == "" {
    fmt.Println("You must supply a queue name (-q QUEUE")
    return
}

if *waitTime < 1 {
    *waitTime = 1
}

if *waitTime > 20 {
    *waitTime = 20
}
```

Initialize a session that the SDK will use to load credentials from the shared credentials file, `~/.aws/credentials` and the default AWS Region from `~/.aws/config`\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

Create a service client and call `CreateQueue`, passing in the time to wait for messages\.

```
svc := sqs.New(sess)

result, err := svc.CreateQueue(&sqs.CreateQueueInput{
    QueueName: queueName,
    Attributes: aws.StringMap(map[string]string{
        "ReceiveMessageWaitTimeSeconds": strconv.Itoa(*waitTime),
    }),
})
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/sqs/CreateLPQueue/CreateLPQueue.go) on GitHub\.

## Enable Long Polling on an Existing Queue<a name="enable-long-polling-on-an-existing-queue"></a>

Create a new Go file named `ConfigureLPQueue.go`\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "flag"
    "fmt"
    "strconv"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/sqs"
)
```

Get the queue name and the optional timeout value from the command line\. Ensure that the wait time is between 1 and 20 seconds\.

```
queue := flag.String("q", "", "The name of the queue")
waitTime := flag.Int("w", 10, "The wait time, in seconds, for long polling")
flag.Parse()

if *queue == "" {
    fmt.Println("You must supply a queue name (-q QUEUE")
    return
}

if *waitTime < 1 {
    *waitTime = 1
}

if *waitTime > 20 {
    *waitTime = 20
}
```

Initialize a session that the SDK will use to load credentials from the shared credentials file, `~/.aws/credentials`, and a default AWS Region from `~/.aws/config`\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

Get the URL of the queue\.

```
svc := sqs.New(sess)

result, err := svc.GetQueueUrl(&sqs.GetQueueUrlInput{
    QueueName: queue,
})
```

The URL is in the QueueUrl property of the returned object\.

```
queueURL := result.QueueUrl
```

Update the queue to enable long polling with a call to `SetQueueAttributes`, passing in the queue URL\.

```
_, err := svc.SetQueueAttributes(&sqs.SetQueueAttributesInput{
    QueueUrl: queueURL,
    Attributes: aws.StringMap(map[string]string{
        "ReceiveMessageWaitTimeSeconds": strconv.Itoa(aws.IntValue(waitTime)),
    }),
})
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/sqs/ConfigureLPQueue/ConfigureLPQueue.go) on GitHub\.

## Enable Long Polling on Message Receipt<a name="enable-long-polling-on-message-receipt"></a>

Create a new Go file named `ReceiveLPMessage.go`\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "flag"
    "fmt"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/sqs"
)
```

Get the queue name and optional visibility and wait time values from the command line\. Ensure that the visibility is between 0 \(zero\) seconds and 12 hours and that the wait time is between 0 and 20 seconds\.

```
queue := flag.String("q", "", "The name of the queue")
waitTime := flag.Int64("w", 10, "How long the queue waits for messages")
flag.Parse()

if *queue == "" {
    fmt.Println("You must supply a queue name (-q QUEUE")
    return
}

if *waitTime < 0 {
    *waitTime = 0
}

if *waitTime > 20 {
    *waitTime = 20
}
```

Initialize a session that the SDK will use to load credentials from the shared credentials file, `~/.aws/credentials` and the default AWS Region from `~/.aws/config`\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

Create a service client and call `GetQueueUrl` to get the URL of the queue\.

```
svc := sqs.New(sess)

result, err := svc.GetQueueUrl(&sqs.GetQueueUrlInput{
    QueueName: queue,
})
```

Call `ReceiveMessage` to get the messages, using long polling, from the queue\.

```
result, err := svc.ReceiveMessage(&sqs.ReceiveMessageInput{
    QueueUrl: queueURL,
    AttributeNames: aws.StringSlice([]string{
        "SentTimestamp",
    }),
    MaxNumberOfMessages: aws.Int64(1),
    MessageAttributeNames: aws.StringSlice([]string{
        "All",
    }),
    WaitTimeSeconds: waitTime,
})
```

Display the IDs of the mesages\.

```
    fmt.Println("Message IDs:")

    for _, msg := range msgs {
        fmt.Println("    " + *msg.MessageId)
    }
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/sqs/ReceiveLPMessage/ReceiveLPMessage.go) on GitHub\.