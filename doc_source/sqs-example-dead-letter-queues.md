# Using Dead Letter Queues in Amazon SQS<a name="sqs-example-dead-letter-queues"></a>

This AWS SDK for Go example shows you how to configure source Amazon SQS queues that send messages to a dead letter queue\.

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/sqs) repository on GitHub\.

## Scenario<a name="sqs-dead-letter-queue-scenario"></a>

A dead letter queue is one that other \(source\) queues can target for messages that can’t be processed successfully\. You can set aside and isolate these messages in the dead letter queue to determine why their processing didn’t succeed\. You must individually configure each source queue that sends messages to a dead letter queue\. Multiple queues can target a single dead letter queue\.

The code uses this method of the Amazon SQS client class:
+  [SetQueueAttributes](https://docs.aws.amazon.com/sdk-for-go/api/service/sqs/#SQS.SetQueueAttributes) 

## Prerequisites<a name="sqs-dead-letter-queue-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with Amazon SQS dead letter queues\. To learn more, see [Using Amazon SQS Dead Letter Queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html) in the Amazon SQS Developer Guide\.

## Configure Source Queues<a name="sqs-example-configure-source-queues"></a>

After you create a queue to act as a dead letter queue, you must configure the other queues that route unprocessed messages to the dead letter queue\. To do this, specify a redrive policy that identifies the queue to use as a dead letter queue and the maximum number of receives by individual messages before they are routed to the dead letter queue\.

Create a new Go file with the name `DeadLetterQueue.go`\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "encoding/json"
    "flag"
    "fmt"
    "strings"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/sqs"
)
```

Get the name of the queue and the dead letter queue from the commmand line\.

```
    queue := flag.String("q", "", "The name of the queue")
    dlQueue := flag.String("d", "", "The name of the dead-letter queue")
    flag.Parse()

    if *queue == "" || *dlQueue == "" {
        fmt.Println("You must supply the names of the queue (-q QUEUE) and the dead-letter queue (-d DLQUEUE)")
        return
    }
```

Initialize a session that the SDK will use to load credentials from the shared credentials file, *\~/\.aws/credentials* and the default AWS Region from *\~/\.aws/config*\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

Create a service client and call `GetQueueUrl` to get the URL for the queue\.

```
svc := sqs.New(sess)

result, err := svc.GetQueueUrl(&sqs.GetQueueUrlInput{
    QueueName: queue,
})
```

The URL of the queue is in the `QueueUrl` property of the returned object\.

```
queueURL := result.QueueUrl
```

Similarly, get the URL of the dead letter queue\.

Create the ARN of the dead\-letter queue from the URL\.

```
parts := strings.Split(*queueURL, "/")
subParts := strings.Split(parts[2], ".")

arn := "arn:aws:" + subParts[0] + ":" + subParts[1] + ":" + parts[3] + ":" + parts[4]
```

Define the redrive policy for the queue\.

```
policy := map[string]string{
    "deadLetterTargetArn": *dlQueueARN,
    "maxReceiveCount":     "10",
}
```

Marshal the policy to use as input for the `SetQueueAttributes` call\.

```
b, err := json.Marshal(policy)
```

Set the policy on the queue\.

```
_, err = svc.SetQueueAttributes(&sqs.SetQueueAttributesInput{
    QueueUrl: queueURL,
    Attributes: map[string]*string{
        sqs.QueueAttributeNameRedrivePolicy: aws.String(string(b)),
    },
})
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/sqs/DeadLetterQueue/DeadLetterQueue.go) on GitHub\.