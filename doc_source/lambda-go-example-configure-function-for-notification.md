# Configuring a Lambda Function to Receive Notifications<a name="lambda-go-example-configure-function-for-notification"></a>

The following example configures the Lambda function `functionName` to accept notifications from the resource with the ARN `sourceArn`\.

Import the packages we use\.

```
import (
    "flag"
    "fmt"
    "os"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/lambda"
)
```

Get the name of the function and ARN of the entity invoking the function\. If either is missing, print an error message and quit\.

```
functionPtr := flag.String("f", "", "The name of the Lambda function")
sourcePtr := flag.String("a", "", "The ARN of the entity invoking the function")
flag.Parse()

if *functionPtr == "" || *sourcePtr == "" {
    fmt.Println("You must supply the name of the function and of the entity invoking the function")
    flag.PrintDefaults()
    os.Exit(1)
}
```

Create the session and Lambda client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

svc := lambda.New(sess)
```

Create the structure for the input argument to the `AddPermission` function\.

```
permArgs := &lambda.AddPermissionInput{
    Action:       aws.String("lambda:InvokeFunction"),
    FunctionName: functionPtr,
    Principal:    aws.String("s3.amazonaws.com"),
    SourceArn:    sourcePtr,
    StatementId:  aws.String("lambda_s3_notification"),
}
```

Finally, call `AddPermission` and display a message with the result of the call\.

```
result, err := svc.AddPermission(permArgs)
if err != nil {
    fmt.Println("Cannot configure function for notifications")
    os.Exit(0)
}

fmt.Println(result)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/lambda/aws-go-sdk-lambda-example-configure-function-for-notification.go) on GitHub\.