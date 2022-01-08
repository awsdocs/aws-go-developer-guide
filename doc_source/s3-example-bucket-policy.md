# Working with Amazon S3 Bucket Policies<a name="s3-example-bucket-policy"></a>

This AWS SDK for Go example shows you how to retrieve, display, and set Amazon S3 bucket polices\. You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/s3) repository on GitHub\.

## Scenario<a name="s3-bucket-policies-scenario"></a>

In this example, you use a series of Go routines to retrieve or set a bucket policy on an Amazon S3 bucket\. The routines use the AWS SDK for Go to configure policy for a selected Amazon S3 bucket using these methods of the Amazon S3 client class:
+  [GetBucketPolicy](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.GetBucketPolicy) 
+  [PutBucketPolicy](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutBucketPolicy) 
+  [DeleteBucketPolicy](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.DeleteBucketPolicy) 

For more information about bucket policies for Amazon S3 buckets, see [Using Bucket Policies and User Policies](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html) in the Amazon S3 Developer Guide\.

## Prerequisites<a name="s3-bucket-policies-prerequisites"></a>
+ You have [set up](setting-up.md), and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with Amazon S3 bucket polices\. To learn more, see [Using Bucket Policies and User Policies](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html) in the Amazon S3 Developer Guide\.

## Retrieve and Display a Bucket Policy<a name="s3-example-get-policy"></a>

Create a new Go file named `s3_get_bucket_policy.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/awserr"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    "bytes"
    "encoding/json"
    "fmt"
    "os"
    "path/filepath"
)
```

Create the `exitError` function to deal with errors\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

This routine prints the policy for a bucket\. If the bucket doesn’t exist, or there was an error, an error message is printed instead\. It requires the bucket name as input\.

```
if len(os.Args) != 2 {
    exitErrorf("bucket name required\nUsage: %s bucket_name",
        filepath.Base(os.Args[0]))
}

bucket := os.Args[1]
```

Initialize a session that the SDK will use to load credentials from the shared credentials file, `~/.aws/credentials`, and create a new S3 service client\.

```
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

svc := s3.New(sess)
```

Call [GetBucketPolicy](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.GetBucketPolicy) to fetch the policy, then display any errors\.

```
result, err := svc.GetBucketPolicy(&s3.GetBucketPolicyInput{
    Bucket: aws.String(bucket),
})
if err != nil {
    // Special error handling for the when the bucket doesn't
    // exists so we can give a more direct error message from the CLI.
    if aerr, ok := err.(awserr.Error); ok {
        switch aerr.Code() {
        case s3.ErrCodeNoSuchBucket:
            exitErrorf("Bucket %q does not exist.", bucket)
        case "NoSuchBucketPolicy":
            exitErrorf("Bucket %q does not have a policy.", bucket)
        }
    }
    exitErrorf("Unable to get bucket %q policy, %v.", bucket, err)
}
```

Use Go’s JSON package to print the Policy JSON returned by the call\.

```
out := bytes.Buffer{}
policyStr := aws.StringValue(result.Policy)
if err := json.Indent(&out, []byte(policyStr), "", "  "); err != nil {
    exitErrorf("Failed to pretty print bucket policy, %v.", err)
}

fmt.Printf("%q's Bucket Policy:\n", bucket)
fmt.Println(out.String())
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_get_bucket_policy.go) on GitHub\.

## Set Bucket Policy<a name="s3-example-set-policy"></a>

This routine sets the policy for a bucket\. If the bucket doesn’t exist, or there was an error, an error message will be printed instead\. It requires the bucket name as input\. It also requires the same Go and AWS SDK for Go packages as the previous example, except for the `bytes` Go package\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/awserr"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    "encoding/json"
    "fmt"
    "os"
    "path/filepath"
)
```

Add the main function and parse the arguments to get the bucket name\.

```
func main() {
    if len(os.Args) != 2 {
        exitErrorf("bucket name required\nUsage: %s bucket_name",
            filepath.Base(os.Args[0]))
    }
    bucket := os.Args[1]
```

Initialize a session that the SDK will use to load configuration, credentials, and region information from the shared credentials file, `~/.aws/credentials`, and create a new S3 service client\.

```
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    svc := s3.New(sess)
```

Create a policy using the map interface, filling in the bucket as the resource\.

```
    readOnlyAnonUserPolicy := map[string]interface{}{
        "Version": "2012-10-17",
        "Statement": []map[string]interface{}{
            {
                "Sid":       "AddPerm",
                "Effect":    "Allow",
                "Principal": "*",
                "Action": []string{
                    "s3:GetObject",
                },
                "Resource": []string{
                    fmt.Sprintf("arn:aws:s3:::%s/*", bucket),
                },
            },
        },
    }
```

Use Go’s JSON package to marshal the policy into a JSON value so that it can be sent to S3\.

```
    policy, err := json.Marshal(readOnlyAnonUserPolicy)
```

Call the S3 client’s [PutBucketPolicy](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutBucketPolicy) to PUT the policy for the bucket and print the results\.

```
    _, err = svc.PutBucketPolicy(&s3.PutBucketPolicyInput{
        Bucket: aws.String(bucket),
        Policy: aws.String(string(policy)),
    })

    fmt.Printf("Successfully set bucket %q's policy\n", bucket)
```

The `exitError` function is used to deal with printing any errors\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_set_bucket_policy.go) on GitHub\.