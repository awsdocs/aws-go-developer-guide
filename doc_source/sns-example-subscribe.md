# Subscribe to an Amazon SNS Topic<a name="sns-example-subscribe"></a>

The following example creates a subscription to the topic with the supplied ARN for the user with the supplied email address in your default region, and displays the resulting ARN\.

```
package main

import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/sns"

    "flag"
    "fmt"
    "os"
)

func main() {
    emailPtr := flag.String("e", "", "The email address of the user subscribing to the topic")
    topicPtr := flag.String("t", "", "The ARN of the topic to which the user subscribes")

    flag.Parse()

    if *emailPtr == "" || *topicPtr == "" {
        fmt.Println("You must supply an email address and topic ARN")
        fmt.Println("Usage: go run SnsSubscribe.go -e EMAIL -t TOPIC-ARN")
        os.Exit(1)
    }

    // Initialize a session that the SDK will use to load
    // credentials from the shared credentials file. (~/.aws/credentials).
    sess := session.Must(session.NewSessionWithOptions(session.Options{
        SharedConfigState: session.SharedConfigEnable,
    }))

    svc := sns.New(sess)

    result, err := svc.Subscribe(&sns.SubscribeInput{
        Endpoint:              emailPtr,
        Protocol:              aws.String("email"),
        ReturnSubscriptionArn: aws.Bool(true), // Return the ARN, even if user has yet to confirm
        TopicArn:              topicPtr,
    })
    if err != nil {
        fmt.Println(err.Error())
        os.Exit(1)
    }

    fmt.Println(*result.SubscriptionArn)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/sns/SnsSubscribe.go) on GitHub\.