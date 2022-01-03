# Using Amazon SQS Queues<a name="sqs-example-create-queue"></a>

These AWS SDK for Go examples show you how to:
+ List Amazon SQS queues
+ Create Amazon SQS queues
+ Get Amazon SQS queue URLs
+ Delete Amazon SQS queues

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/sqs) repository on GitHub\.

## Scenario<a name="sqs-create-scenario"></a>

These examples demonstrate how to work with Amazon SQS queues\.

The code uses these methods of the Amazon SQS client class:
+  [CreateQueue](https://docs.aws.amazon.com/sdk-for-go/api/service/sqs/#SQS.CreateQueue) 
+  [ListQueues](https://docs.aws.amazon.com/sdk-for-go/api/service/sqs/#SQS.ListQueues) 
+  [GetQueueUrl](https://docs.aws.amazon.com/sdk-for-go/api/service/sqs/#SQS.GetQueueUrl) 
+  [DeleteQueue](https://docs.aws.amazon.com/sdk-for-go/api/service/sqs/#SQS.DeleteQueue) 

## Prerequisites<a name="sqs-create-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with using Amazon SQS\. To learn more, see [How Queues Work](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-how-it-works.html) in the Amazon SQS Developer Guide\.

## List Queues<a name="sqs-example-list-queues"></a>

Create a new Go file named `ListQueues.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "fmt"

    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/sqs"
)
```

Initialize a session that the SDK uses to load credentials from the shared credentials file, \~/\.aws/credentials and the default region from \~/\.aws/config\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

Create a service client and call `ListQueues`, passing in `nil` to return all queues\.

```
svc := sqs.New(sess)

result, err := svc.ListQueues(nil)
```

Loop through the queue URLs to print them\.

```
for i, url := range result.QueueUrls {
    fmt.Printf("%d: %s\n", i, *url)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/ListQueues/ListQueues.go) on GitHub\.

## Create a Queue<a name="create-a-queue"></a>

Create a new Go file named `CreateQueue.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "flag"
    "fmt"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/sqs"
)
```

Get the queue name from the command line\.

```
queue := flag.String("q", "", "The name of the queue")
flag.Parse()

if *queue == "" {
    fmt.Println("You must supply a queue name (-q QUEUE")
    return
}
```

Initialize a session that the SDK uses to load credentials from the shared credentials file, \~/\.aws/credentials and the default region from \~/\.aws/config\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

Create a service client and call `CreateQueue`, passing in the queue name and queue attributes\.

```
svc := sqs.New(sess)

result, err := svc.CreateQueue(&sqs.CreateQueueInput{
    QueueName: queue,
    Attributes: map[string]*string{
        "DelaySeconds":           aws.String("60"),
        "MessageRetentionPeriod": aws.String("86400"),
    },
})
```

Display the queue URL\.

```
fmt.Println("URL: " + *result.QueueUrl)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/CreateQueue/CreateQueue.go) on GitHub\.

## Get the URL of a Queue<a name="get-the-url-of-a-queue"></a>

Create a new Go file named `GetQueueUrl.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "flag"
    "fmt"

    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/sqs"
)
```

Get the name of the queue from the command line\.

```
queue := flag.String("q", "", "The name of the queue")
flag.Parse()

if *queue == "" {
    fmt.Println("You must supply a queue name (-q QUEUE")
    return
}
```

Initialize a session that the SDK uses to load credentials from the shared credentials file, \~/\.aws/credentials and the default region from \~/\.aws/config\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

Create a service client and call `GetQueueUrl`, passing in the queue name\.

```
svc := sqs.New(sess)

result, err := svc.GetQueueUrl(&sqs.GetQueueUrlInput{
    QueueName: queue,
})
```

Display the URL of the queue\.

```
fmt.Println("URL: " + *result.QueueUrl)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/GetQueueURL/GetQueueURL.go) on GitHub\.

## Delete a Queue<a name="delete-a-queue"></a>

Create a new Go file named `DeleteQueue.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "flag"
    "fmt"

    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/sqs"
)
```

Get the name of the queue from the command line\.

```
queue := flag.String("q", "", "The name of the queue")
flag.Parse()

if *queue == "" {
    fmt.Println("You must supply a queue name (-q QUEUE")
    return
}
```

Initialize a session that the SDK uses to load credentials from the shared credentials file, \~/\.aws/credentials and the default region from \~/\.aws/config\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

Create a service client and all `DeleteQueue` passing in the queue name\.

```
svc := sqs.New(sess)

result, err := svc.GetQueueUrl(&sqs.GetQueueUrlInput{
    QueueName: queueName,
})
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/DeleteQueue/DeleteQueue.go) on GitHub\.