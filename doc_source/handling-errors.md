# Handling Errors in the AWS SDK for Go<a name="handling-errors"></a>

The AWS SDK for Go returns errors that satisfy the Go `error` interface type and the [Error](https://docs.aws.amazon.com/sdk-for-go/api/aws/awserr/#Error) interface in the `aws/awserr` package\. You can use the `Error()` method to get a formatted string of the SDK error message without any special handling\.

```
if err != nil {
    if awsErr, ok := err.(awserr.Error); ok {
        // process SDK error
    }
}
```

Errors returned by the SDK are backed by a concrete type that will satisfy the `awserr.Error` interface\. The interface has the following methods, which provide classification and information about the error\.
+  `Code` returns the classification code by which related errors are grouped\.
+  `Message` returns a description of the error\.
+  `OrigErr` returns the original error of type `error` that is wrapped by the `awserr.Error` interface, such as a standard library error or a service error\.

## Handling Specific Service Error Codes<a name="handling-specific-service-error-codes"></a>

The following example demonstrates how to handle error codes that you encounter while using the AWS SDK for Go\. The example assumes you have already set up and configured the SDK \(that is, all required packages are imported and your credentials and region are set\)\. For more information, see [Getting Started with the AWS SDK for Go](setting-up.md) and [Configuring the AWS SDK for Go](configuring-sdk.md)\.

This example highlights how you can use the *awserr\.Error* type to perform logic based on specific error codes returned by service API operations\.

In this example the *S3* *GetObject* API operation is used to request the contents of an object in S3\. The example handles the *NoSuchBucket* and *NoSuchKey* error codes, printing custom messages to stderr\. If any other error is received, a generic message is printed\.

```
svc := s3.New(sess)
resp, err := svc.GetObject(&s3.GetObjectInput{
    Bucket: aws.String(os.Args[1]),
    Key:    aws.String(os.Args[2]),
})

if err != nil {
    // Casting to the awserr.Error type will allow you to inspect the error
    // code returned by the service in code. The error code can be used
    // to switch on context specific functionality. In this case a context
    // specific error message is printed to the user based on the bucket
    // and key existing.
    //
    // For information on other S3 API error codes see:
    // http://docs.aws.amazon.com/AmazonS3/latest/API/ErrorResponses.html
    if aerr, ok := err.(awserr.Error); ok {
        switch aerr.Code() {
        case s3.ErrCodeNoSuchBucket:
            exitErrorf("bucket %s does not exist", os.Args[1])
        case s3.ErrCodeNoSuchKey:
            exitErrorf("object with key %s does not exist in bucket %s", os.Args[2], os.Args[1])
        }
    }
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/extending_sdk/handleServiceErrorCodes.go) on GitHub\.

## Additional Error Information<a name="additional-error-information"></a>

In addition to the `awserr.Error` interface, you might be able to use other interfaces to get more information about an error\.

### Specific Error Interfaces<a name="specific-error-interfaces"></a>

Other packages might provide their own error interfaces\. For example, the [service/s3/s3manager](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3manager) package provides a [MultiUploadFailure](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3manager/#MultiUploadFailure) interface to retrieve the upload ID\. This is helpful when you must manually clean up a failed multi\-part upload\.

```
output, err := s3manager.Upload(svc, input, opts)
if err != nil {
    if multierr, ok := err.(MultiUploadFailure); ok {
        // Process error and its associated uploadID
        fmt.Println("Error:", multierr.Code(), multierr.Message(), multierr.UploadID())
    } else {
        // Process error generically
        fmt.Println("Error:", err.Error())
    }
}
```

For more information, see the [s3Manager\.MultiUploadFailure](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3manager/#MultiUploadFailure) interface in the AWS SDK for Go API Reference\.