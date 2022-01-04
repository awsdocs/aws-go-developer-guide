# Managing Visibility Timeout in Amazon SQS Queues<a name="sqs-example-managing-visibility-timeout"></a>

This AWS SDK for Go example shows you how to:
+ Change visibility timeout with Amazon SQS queues

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/sqs) repository on GitHub\.

## Scenario<a name="sqs-visibility-scenario"></a>

This example manages visibility timeout with Amazon SQS queues\. Visibility is the duration, in seconds, while messages are in the queue, but not available to other consumers\. It uses these methods of the Amazon SQS client class:
+  [ChangeMessageVisibility](https://docs.aws.amazon.com/sdk-for-go/api/service/sqs/#SQS.ChangeMessageVisibility) 
+  [GetQueueUrl](https://docs.aws.amazon.com/sdk-for-go/api/service/sqs/#SQS.GetQueueUrl) 

## Prerequisites<a name="sqs-visibility-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with using Amazon SQS visibility timeout\. To learn more, see [Visibility Timeout](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html) in the Amazon SQS Developer Guide\.

## Change the Visibility Timeout<a name="sqs-example-visibility-timeout"></a>

Create a new Go file named `ChangeMsgVisibility.go`\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "flag"
    "fmt"

    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/sqs"
)
```

Get the queue name, receipt handle of the message, and visibility duration from the command line\. Ensure the visibility is from 0 \(zero\) seconds to 12 hours\.

```
queue := flag.String("q", "", "The name of the queue")
handle := flag.String("h", "", "The receipt handle of the message")
visibility := flag.Int64("v", 30, "The duration, in seconds, that the message is not visible to other consumers")
flag.Parse()

if *queue == "" || *handle == "" {
    fmt.Println("You must supply a queue name (-q QUEUE) and message receipt handle (-h HANDLE)")
    return
}

if *visibility < 0 {
    *visibility = 0
}

if *visibility > 12*60*60 {
    *visibility = 12 * 60 * 60
}
```

Initialize a session that the SDK will use to load credentials from the shared credentials file, \~/\.aws/credentials and the default region from \~/\.aws/config\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

Create a service client and call `GetQueueUrl` to retrieve the URL of the queue\.

```
svc := sqs.New(sess)

result, err := svc.GetQueueUrl(&sqs.GetQueueUrlInput{
    QueueName: queue,
})
```

The URL of the queue is in the `QueueUrl` property of the returned object\.

```
queueURL := urlResult.QueueUrl
```

Call `ChangeMessageVisibility` to change the visibility of the messages in the queue\.

```
_, err := svc.ChangeMessageVisibility(&sqs.ChangeMessageVisibilityInput{
    ReceiptHandle:     handle,
    QueueUrl:          queueURL,
    VisibilityTimeout: visibility,
})
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/sqs/ChangeMsgVisibility/ChangeMsgVisibility.go) on GitHub\.