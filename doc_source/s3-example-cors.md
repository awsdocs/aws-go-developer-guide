# Working with Amazon S3 CORS Permissions<a name="s3-example-cors"></a>

This AWS SDK for Go example shows you how to list Amazon S3 buckets and configure CORS and bucket logging\. You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/s3) repository on GitHub\.

## Scenario<a name="s3-cors-scenario"></a>

In this example, a series of Go routines are used to list your Amazon S3 buckets and to configure CORS and bucket logging\. The routines use the AWS SDK for Go to configure a selected Amazon S3 bucket using these methods of the Amazon S3 client class:
+  [GetBucketCors](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.GetBucketCors) 
+  [PutBucketCors](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutBucketCors) 

If you are unfamiliar with using CORS configuration with an Amazon S3 bucket, it’s worth your time to read [Cross\-Origin Resource Sharing \(CORS\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html) in the Amazon S3 Developer Guide before proceeding\.

## Prerequisites<a name="s3-cors-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with using CORS configuration with an Amazon S3 bucket\. To learn more, see [Cross\-Origin Resource Sharing \(CORS\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html) in the Amazon S3 Developer Guide\.

## Configure CORS on the Bucket<a name="s3-example-cors-config"></a>

Create a new Go file named `s3_set_cors.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "flag"
    "fmt"
    "os"
    "strings"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
)
```

This routine configures CORS rules for a bucket by setting the allowed HTTP methods\.

It requires the bucket name and can also take a space\-separated list of HTTP methods\. Using the Go Language’s `flag` package, it parses the input and validates the bucket name\.

```
bucketPtr := flag.String("b", "", "Bucket to set CORS on, (required)")

flag.Parse()

if *bucketPtr == "" {
    exitErrorf("-b <bucket> Bucket name required")
}

methods := filterMethods(flag.Args())
```

Notice the helper method, `filterMethods`, which ensures the methods passed in are uppercase\.

```
func filterMethods(methods []string) []string {
    filtered := make([]string, 0, len(methods))
    for _, m := range methods {
        v := strings.ToUpper(m)
        switch v {
        case "POST", "GET", "PUT", "PATCH", "DELETE":
            filtered = append(filtered, v)
        }
    }

    return filtered
}
```

Initialize a session that the SDK will use to load credentials, from the shared credentials file, \~/\.aws/credentials, and create a new S3 service client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

svc := s3.New(sess)
```

Create a new [CORSRule](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#CORSRule) to set up the CORS configuration\.

```
rule := s3.CORSRule{
    AllowedHeaders: aws.StringSlice([]string{"Authorization"}),
    AllowedOrigins: aws.StringSlice([]string{"*"}),
    MaxAgeSeconds:  aws.Int64(3000),

    // Add HTTP methods CORS request that were specified in the CLI.
    AllowedMethods: aws.StringSlice(methods),
}
```

Add the `CORSRule` to the `PutBucketCorsInput` structure, call `PutBucketCors` with that structure, and print a success or error message\.

```
params := s3.PutBucketCorsInput{
    Bucket: bucketPtr,
    CORSConfiguration: &s3.CORSConfiguration{
        CORSRules: []*s3.CORSRule{&rule},
    },
}

_, err := svc.PutBucketCors(&params)
if err != nil {
    // Print the error message
    exitErrorf("Unable to set Bucket %q's CORS, %v", *bucketPtr, err)
}

// Print the updated CORS config for the bucket
fmt.Printf("Updated bucket %q CORS for %v\n", *bucketPtr, methods)
```

The example uses the following error function\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```