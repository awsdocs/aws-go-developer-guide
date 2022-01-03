# Using an Amazon S3 Bucket as a Static Web Host<a name="s3-example-static-web-host"></a>

This AWS SDK for Go example shows you how to configure an Amazon S3 bucket to act as a static web host\. You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/s3) repository on GitHub\.

## Scenario<a name="s3-website-scenario"></a>

In this example, you use a series of Go routines to configure any of your buckets to act as a static web host\. The routines use the AWS SDK for Go to configure a selected Amazon S3 bucket using these methods of the Amazon S3 client class:
+ GetBucketWebsite
+ PutBucketWebsite
+ DeleteBucketWebsite

For more information about using an Amazon S3 bucket as a static web host, see [Hosting a Static Website on Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) in the Amazon S3 Developer Guide\.

## Prerequisites<a name="s3-website-prerequisites"></a>
+ You have [set up](setting-up.md), and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with hosting static websites on Amazon S3\. To learn more, see [Hosting a Static Website on Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) in the Amazon S3 Developer Guide\.

## Retrieve a Bucket’s Website Configuration<a name="s3-example-retrieve-config"></a>

Create a new Go file named `s3_get_bucket_website.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/awserr"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    "fmt"
    "os"
)
```

This routine requires you to pass in an argument containing the name of the bucket for which you want to get website configuration\.

```
if len(os.Args) != 2 {
    exitErrorf("bucket name required\nUsage: %s bucket_name", os.Args[0])
}

bucket := os.Args[1]
```

Initialize a session that the SDK will use to load credentials from the shared credentials file, \~/\.aws/credentials, and create a new S3 service client\.

```
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Create S3 service client
svc := s3.New(sess)
```

Call [GetBucketWebsite](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.GetBucketWebsite) to get the bucket configuration\. You should check for the `NoSuchWebsiteConfiguration` error code, which tells you that the bucket doesn’t have a website configured\.

```
result, err := svc.GetBucketWebsite(&s3.GetBucketWebsiteInput{
    Bucket: aws.String(bucket),
})
if err != nil {
    // Check for the NoSuchWebsiteConfiguration error code telling us
    // that the bucket does not have a website configured.
    if awsErr, ok := err.(awserr.Error); ok && awsErr.Code() == "NoSuchWebsiteConfiguration" {
        exitErrorf("Bucket %s does not have website configuration\n", bucket)
    }
    exitErrorf("Unable to get bucket website config, %v", err)
}
```

Print the bucket’s website configuration\.

```
fmt.Println("Bucket Website Configuration:")
fmt.Println(result)
```

## Set a Bucket’s Website Configuration<a name="s3-example-set-bucket-website"></a>

Create a Go file named `s3_set_bucket_website.go` and add the code below\. The Amazon S3[PutBucketWebsite](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutBucketWebsite) operation sets the website configuration on a bucket, replacing any existing configuration\.

Create a new Go file named `s3_create_bucket.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    "fmt"
    "os"
    "path/filepath"
)
```

This routine requires you to pass in an argument containing the name of the bucket and the index suffix page\.

```
if len(os.Args) != 4 {
    exitErrorf("bucket name and index suffix page required\nUsage: %s bucket_name index_page [error_page]",
        filepath.Base(os.Args[0]))
}

bucket := fromArgs(os.Args, 1)
indexSuffix := fromArgs(os.Args, 2)
errorPage := fromArgs(os.Args, 3)
```

Initialize a session that the SDK will use to load configuration, credentials, and region information from the shared credentials file, \~/\.aws/credentials, and create a new S3 service client\.

```
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Create S3 service client
svc := s3.New(sess)
```

Create the parameters to be passed in to [PutBucketWebsite](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutBucketWebsite), including the bucket name and index document details\. If the user passed in an error page when calling the routine, also add that to the parameters\.

```
params := s3.PutBucketWebsiteInput{
    Bucket: aws.String(bucket),
    WebsiteConfiguration: &s3.WebsiteConfiguration{
        IndexDocument: &s3.IndexDocument{
            Suffix: aws.String(indexSuffix),
        },
    },
}

// Add the error page if set on CLI
if len(errorPage) > 0 {
    params.WebsiteConfiguration.ErrorDocument = &s3.ErrorDocument{
        Key: aws.String(errorPage),
    }
}
```

Call `PutBucketWebsite`, passing in the parameters you just defined\. If an error occurs, print the errordetails and exit the routine\.

```
_, err = svc.PutBucketWebsite(&params)
if err != nil {
    exitErrorf("Unable to set bucket %q website configuration, %v",
        bucket, err)
}

fmt.Printf("Successfully set bucket %q website configuration\n", bucket)
```

## Delete Website Configuration on a Bucket<a name="s3-example-delete-website"></a>

Create a Go file named `s3_delete_bucket_website.go`\. Import the relevant Go and AWS SDK for Go packages\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    "fmt"
    "os"
    "path/filepath"
)
```

This routine requires you to pass in the name of the bucket for which you want to delete the website configuration\.

```
if len(os.Args) != 2 {
    exitErrorf("bucket name required\nUsage: %s bucket_name",
        filepath.Base(os.Args[0]))
}

bucket := os.Args[1]
```

Initialize a session that the SDK will use to load configuration, credentials, and region information from the shared credentials file, \~/\.aws/credentials, and create a new S3 service client\.

```
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Create S3 service client
svc := s3.New(sess)
```

Call `DeleteBucketWebsite` and pass in the name of the bucket to complete the action\.

```
_, err = svc.DeleteBucketWebsite(&s3.DeleteBucketWebsiteInput{
    Bucket: aws.String(bucket),
})
if err != nil {
    exitErrorf("Unable to delete bucket %q website configuration, %v",
        bucket, err)
}

fmt.Printf("Successfully delete bucket %q website configuration\n", bucket)
```