# Creating a Lambda Function<a name="lambda-go-example-create-function"></a>

The following example creates the Lambda function `functionName` in the `us-west-2` region using the following values:
+ Role ARN: `resourceArn`\. In most cases, you need to attach only the `AWSLambdaExecute` managed policy to the policy for this role\.
+ Function entry point: `handler` 
+ Runtime: `runtime` 
+ Zip file: `zipFileName` \+ `.zip` 
+ Bucket: `bucketName` 
+ Key: `zipFileName` 

The first step is to import the packages we use in the example\.

```
import (
    "flag"
    "fmt"
    "io/ioutil"

    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/lambda"
)
```

Get the zip file name, bucket name, function name, handler, ARN, and runtime values from the command line\. If any are missing, print and error message and quit\.

```
zipFile := flag.String("z", "", "The name of the ZIP file, without the .zip extension.")
bucket := flag.String("b", "", "the name of bucket to which the ZIP file is uploaded.")
function := flag.String("f", "", "The name of the Lambda function.")
handler := flag.String("h", "main", "The name of the package.class handling the call.")
roleARN := flag.String("a", "", "The ARN of the role that calls the function.")
runtime := flag.String("r", "go1.x", "The runtime for the function.")

flag.Parse()

if *zipFile == "" || *bucket == "" || *function == "" || *handler == "" || *roleARN == "" || *runtime == "" {
    fmt.Println("You must supply a zip file name, bucket name, function name, handler (package) name, role ARN, and runtime value.")
    return
}
```

Create the session and Lambda client and get the contents of the zip file\. If you cannot read the zip file contents, print an error message and quit\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

Next, create the structures for the input argument to the `CreateFunction` function\.

```
createCode := &lambda.FunctionCode{
    //      S3Bucket:        bucket,
    //      S3Key:           zipFile,
    //      S3ObjectVersion: aws.String("1"),
    ZipFile: contents,
}

createArgs := &lambda.CreateFunctionInput{
    Code:         createCode,
    FunctionName: function,
    Handler:      handler,
    Role:         role,
    Runtime:      runtime,
}
```

Finally, call `CreateFunction` and display a message with the result of the call\.

```
result, err := svc.CreateFunction(createArgs)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/lambda/aws-go-sdk-lambda-example-create-function.go) on GitHub\.