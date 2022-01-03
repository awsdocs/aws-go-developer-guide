# Sending and Receiving Messages in Amazon SQS<a name="sqs-example-receive-message"></a>

These AWS SDK for Go examples show you how to:
+ Send a message to an Amazon SQS queue
+ Receive a message from an Amazon SQS queue
+ Delete a message from an Amazon SQS queue

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/sqs) repository on GitHub\.

## The Scenario<a name="sqs-receive-message-scenario"></a>

These examples demonstrate sending, receiving, and deleting messages from an Amazon SQS queue\.

The code uses these methods of the Amazon SQS client class:
+  [SendMessage](https://docs.aws.amazon.com/sdk-for-go/api/service/sqs/#SQS.SendMessage) 
+  [ReceiveMessage](https://docs.aws.amazon.com/sdk-for-go/api/service/sqs/#SQS.ReceiveMessage) 
+  [DeleteMessage](https://docs.aws.amazon.com/sdk-for-go/api/service/sqs/#SQS.DeleteMessage) 
+  [GetQueueUrl](https://docs.aws.amazon.com/sdk-for-go/api/service/sqs/#SQS.GetQueueUrl) 

## Prerequisites<a name="sqs-receive-message-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with the details of Amazon SQS messages\. To learn more, see [Sending a Message to an Amazon SQS Queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-send-message.html) and [Receiving and Deleting a Message from an Amazon SQS Queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-receive-delete-message.html) in the Amazon SQS Developer Guide\.

## Send a Message to a Queue<a name="sqs-example-send-message"></a>

Create a new Go file named `SendMessage.go`\.

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

Get the name of the queue from the command line\.

```
queue := flag.String("q", "", "The name of the queue")
flag.Parse()

if *queue == "" {
    fmt.Println("You must supply the name of a queue (-q QUEUE)")
    return
}
```

Initialize a session that the SDK uses to load credentials from the shared credentials file, \~/\.aws/credentials and the default region from \~/\.aws/config\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

Create a service client and call *SendMessage*\. The input represents information about a fiction best seller for a particular week and defines title, author, and weeks on the list values\.

```
svc := sqs.New(sess)

_, err := svc.SendMessage(&sqs.SendMessageInput{
    DelaySeconds: aws.Int64(10),
    MessageAttributes: map[string]*sqs.MessageAttributeValue{
        "Title": &sqs.MessageAttributeValue{
            DataType:    aws.String("String"),
            StringValue: aws.String("The Whistler"),
        },
        "Author": &sqs.MessageAttributeValue{
            DataType:    aws.String("String"),
            StringValue: aws.String("John Grisham"),
        },
        "WeeksOn": &sqs.MessageAttributeValue{
            DataType:    aws.String("Number"),
            StringValue: aws.String("6"),
        },
    },
    MessageBody: aws.String("Information about current NY Times fiction bestseller for week of 12/11/2016."),
    QueueUrl:    queueURL,
})
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/SendMessage/SendMessage.go) on GitHub\.

## Receiving a Message from a Queue<a name="sqs-example-receive-mesage"></a>

Create a new Go file named `ReceiveMessage.go`\.

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

Get the name of the queue and how long the message is hidden from other consumers from the command line\.

```
queue := flag.String("q", "", "The name of the queue")
timeout := flag.Int64("t", 5, "How long, in seconds, that the message is hidden from others")
flag.Parse()

if *queue == "" {
    fmt.Println("You must supply the name of a queue (-q QUEUE)")
    return
}

if *timeout < 0 {
    *timeout = 0
}

if *timeout > 12*60*60 {
    *timeout = 12 * 60 * 60
}
```

Initialize a session that the SDK uses to load credentials from the shared credentials file, \~/\.aws/credentials and the default region from \~/\.aws/config\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

Create a service client and call the *GetQueueUrl* function to get the URL of the queue\.

```
svc := sqs.New(sess)

urlResult, err := svc.GetQueueUrl(&sqs.GetQueueUrlInput{
    QueueName: queue,
})
```

The URL of the queue is in the *QueueUrl* property of the returned object\.

```
queueURL := urlResult.QueueUrl
```

Call the *ReceiveMessage* function to get the messages in the queue\.

```
msgResult, err := svc.ReceiveMessage(&sqs.ReceiveMessageInput{
    AttributeNames: []*string{
        aws.String(sqs.MessageSystemAttributeNameSentTimestamp),
    },
    MessageAttributeNames: []*string{
        aws.String(sqs.QueueAttributeNameAll),
    },
    QueueUrl:            queueURL,
    MaxNumberOfMessages: aws.Int64(1),
    VisibilityTimeout:   timeout,
})
```

Print the message handle of the first message in the queue \(you need the handle to delete the message\)\.

```
fmt.Println("Message Handle: " + *msgResult.Messages[0].ReceiptHandle)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/ReceiveMessage/ReceiveMessage.go) on GitHub\.

## Delete a Message from a Queue<a name="sqs-example-delete-message"></a>

Create a new Go file named `DeleteMessage.go`\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "flag"
    "fmt"

    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/sqs"
)
```

Get the name of the queue and the handle for the message from the command line\.

```
queue := flag.String("q", "", "The name of the queue")
messageHandle := flag.String("m", "", "The receipt handle of the message")
flag.Parse()

if *queue == "" || *messageHandle == "" {
    fmt.Println("You must supply a queue name (-q QUEUE) and message receipt handle (-m MESSAGE-HANDLE)")
    return
}
```

Initialize a session that the SDK uses to load credentials from the shared credentials file, \~/\.aws/credentials and the default region from \~/\.aws/config\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

Create a service client and call the *DeleteMessage* function, passing in the name of the queue and the message handle\.

```
svc := sqs.New(sess)

_, err := svc.DeleteMessage(&sqs.DeleteMessageInput{
    QueueUrl:      queueURL,
    ReceiptHandle: messageHandle,
})
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/DeleteMessage/DeleteMessage.go) on GitHub\.