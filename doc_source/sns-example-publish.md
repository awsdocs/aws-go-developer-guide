# Sending a Message to All Amazon SNS Topic Subscribers<a name="sns-example-publish"></a>

The following example sends the message supplied on the command line to all subscribers to the Amazon SNS topic with the ARN specified on the command line\.

```
package main

import (
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/sns"

    "flag"
    "fmt"
    "os"
)

func main() {
    msgPtr := flag.String("m", "", "The message to send to the subscribed users of the topic")
    topicPtr := flag.String("t", "", "The ARN of the topic to which the user subscribes")

    flag.Parse()

    if *msgPtr == "" || *topicPtr == "" {
        fmt.Println("You must supply a message and topic ARN")
        fmt.Println("Usage: go run SnsPublish.go -m MESSAGE -t TOPIC-ARN")
        os.Exit(1)
    }

    // Initialize a session that the SDK will use to load
    // credentials from the shared credentials file. (~/.aws/credentials).
    sess := session.Must(session.NewSessionWithOptions(session.Options{
        SharedConfigState: session.SharedConfigEnable,
    }))

    svc := sns.New(sess)

    result, err := svc.Publish(&sns.PublishInput{
        Message:  msgPtr,
        TopicArn: topicPtr,
    })
    if err != nil {
        fmt.Println(err.Error())
        os.Exit(1)
    }

    fmt.Println(*result.MessageId)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/sns/SnsPublish.go) on GitHub\.