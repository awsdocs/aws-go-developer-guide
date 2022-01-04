# Listing Your Amazon SNS Topics<a name="sns-example-list-topics"></a>

The following example lists the ARNs of your Amazon SNS topics in your default region\.

```
package main

import (
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/sns"

    "fmt"
    "os"
)

func main() {
    // Initialize a session that the SDK will use to load
    // credentials from the shared credentials file. (~/.aws/credentials).
    sess := session.Must(session.NewSessionWithOptions(session.Options{
        SharedConfigState: session.SharedConfigEnable,
    }))

    svc := sns.New(sess)

    result, err := svc.ListTopics(nil)
    if err != nil {
        fmt.Println(err.Error())
        os.Exit(1)
    }

    for _, t := range result.Topics {
        fmt.Println(*t.TopicArn)
    }
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/sns/SnsListTopics.go) on GitHub\.