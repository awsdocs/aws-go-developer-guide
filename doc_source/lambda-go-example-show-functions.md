# Displaying Information about All Lambda Functions<a name="lambda-go-example-show-functions"></a>

First import the packages we use in this example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/lambda"

    "fmt"
    "os"
)
```

Next, create the session and Lambda client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

svc := lambda.New(sess, &aws.Config{Region: aws.String("us-west-2")})
```

Next, call `ListFunctions` and exit if there is an error\.

```
result, err := svc.ListFunctions(nil)
if err != nil {
    fmt.Println("Cannot list functions")
    os.Exit(0)
}
```

Finally, display the names and descriptions of the Lambda functions\.

```
fmt.Println("Functions:")

for _, f := range result.Functions {
    fmt.Println("Name:        " + aws.StringValue(f.FunctionName))
    fmt.Println("Description: " + aws.StringValue(f.Description))
    fmt.Println("")
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/lambda/aws-go-sdk-lambda-example-show-functions.go) on GitHub\.