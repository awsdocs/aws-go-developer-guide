# Getting a List of Voices<a name="polly-example-describe-voices"></a>

This example uses the [DescribeVoices](https://docs.aws.amazon.com/sdk-for-go/api/service/polly/#Polly.DescribeVoices) operation to get the list of voices in the `us-west-2` region\.

Choose `Copy` to save the code locally\.

Create the file *pollyDescribeVoices\.go*\. Import the packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/polly"

    "fmt"
    "os"
)
```

Initialize a session that the SDK will use to load credentials from the shared credentials file `~/.aws/credentials`, load your configuration from the shared configuration file `~/.aws/config`, and create an Amazon Polly client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

svc := polly.New(sess)
```

Create the input for and call `DescribeVoices`\.

```
input := &polly.DescribeVoicesInput{LanguageCode: aws.String("en-US")}
resp, err := svc.DescribeVoices(input)
```

Display the name and gender of the voices\.

```
for _, v := range resp.Voices {
    fmt.Println("Name:   " + *v.Name)
    fmt.Println("Gender: " + *v.Gender)
    fmt.Println("")
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/polly/pollyDescribeVoices.go) on GitHub\.