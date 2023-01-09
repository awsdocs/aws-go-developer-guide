# List Your Amazon SNS Subscriptions<a name="sns-example-list-subscriptions"></a>

The following example lists the ARNs for your Amazon SNS topic subscriptions and the associated topic in your default region\.

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
    // Initialize a session that the SDK will use to load
	// credentials from the shared credentials file. (~/.aws/credentials).
	sess := session.Must(session.NewSessionWithOptions(session.Options{
		SharedConfigState: session.SharedConfigEnable,
	}))

	svc := sns.New(sess)

	result, err := svc.ListSubscriptions(&sns.ListSubscriptionsInput{})
	if err != nil {
		fmt.Println(err.Error())
		os.Exit(1)
	}

	for _, t := range result.Subscriptions {
		fmt.Println(*t.SubscriptionArn)
	}

	fmt.Println(*result.NextToken)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/sns/SnsListSubscriptions.go) on GitHub\.
