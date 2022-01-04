# Performing Basic Amazon S3 Bucket Operations<a name="s3-example-basic-bucket-operations"></a>

These AWS SDK for Go examples show you how to perform the following operations on Amazon S3 buckets and bucket items:
+ List the buckets in your account
+ Create a bucket
+ List the items in a bucket
+ Upload a file to a bucket
+ Download a bucket item
+ Copy a bucket item to another bucket
+ Delete a bucket item
+ Delete all the items in a bucket
+ Restore a bucket item
+ Delete a bucket
+ List the users with administrator privileges

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/s3) repository on GitHub\.

## Scenario<a name="s3-examples-bucket-ops-scenario"></a>

In these examples, a series of Go routines are used to perform operations on your Amazon S3 buckets\. The routines use the AWS SDK for Go to perform Amazon S3 bucket operations using the following methods of the Amazon S3 client class, unless otherwise noted:
+  [ListBuckets](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.ListBuckets) 
+  [CreateBucket](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.CreateBucket) 
+  [ListObjects](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.ListObjects) 
+  [Upload](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3manager/#Uploader.Upload) \(from the **s3manager\.NewUploader** class\)
+  [Download](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3manager/#Downloader.Download) \(from the **s3manager\.NewDownloader** class\)
+  [CopyObject](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.CopyObject) 
+  [DeleteObject](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.DeleteObject) 
+  [DeleteObjects](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.DeleteObjects) 
+  [RestoreObject](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.RestoreObject) 
+  [DeleteBucket](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.DeleteBucket) 

## Prerequisites<a name="s3-examples-bucket-ops-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with buckets\. To learn more, see [Working with Amazon S3 Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html) in the Amazon S3 Developer Guide\.

## List Buckets<a name="s3-examples-bucket-ops-list-buckets"></a>

The [ListBuckets](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.ListBuckets) function lists the buckets in your account\.

The following example lists the buckets in your account\. There are no command line arguments\.

Create the file *s3\_list\_buckets\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    "fmt"
    "os"
)
```

Create a function we use to display errors and exit\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `~/.aws/credentials`, and create a new Amazon S3 service client\.

```
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Create S3 service client
svc := s3.New(sess)
```

Call [ListBuckets](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.ListBuckets)\. Passing `nil` means no filters are applied to the returned list\. If an error occurs, call `exitErrorf`\. If no error occurs, loop through the buckets, printing the name and creation date of each bucket\.

```
result, err := svc.ListBuckets(nil)
if err != nil {
    exitErrorf("Unable to list buckets, %v", err)
}

fmt.Println("Buckets:")

for _, b := range result.Buckets {
    fmt.Printf("* %s created on %s\n",
        aws.StringValue(b.Name), aws.TimeValue(b.CreationDate))
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_list_buckets.go) on GitHub\.

## Create a Bucket<a name="s3-examples-bucket-ops-create-bucket"></a>

The [CreateBucket](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.CreateBucket) function creates a bucket in your account\.

The following example creates a bucket with the name specified as a command line argument\. You must specify a globally unique name for the bucket\.

Create the file *s3\_create\_bucket\.go*\. Import the following Go and AWS SDK for Go packages\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    "fmt"
    "os"
)
```

Create a function we use to display errors and exit\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

The program requires one argument, the name of the bucket to create\.

```
if len(os.Args) != 2 {
    exitErrorf("Bucket name missing!\nUsage: %s bucket_name", os.Args[0])
}

bucket := os.Args[1]
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `~/.aws/credentials`, and create a new S3 service client\.

```
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Create S3 service client
svc := s3.New(sess)
```

Call [CreateBucket](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.CreateBucket), passing in the bucket name defined previously\. If an error occurs, call `exitErrorf`\. If there are no errors, wait for a notification that the bucket was created\.

```
_, err = svc.CreateBucket(&s3.CreateBucketInput{
    Bucket: aws.String(bucket),
})
if err != nil {
    exitErrorf("Unable to create bucket %q, %v", bucket, err)
}

// Wait until bucket is created before finishing
fmt.Printf("Waiting for bucket %q to be created...\n", bucket)

err = svc.WaitUntilBucketExists(&s3.HeadBucketInput{
    Bucket: aws.String(bucket),
})
```

If the `WaitUntilBucketExists` call returns an error, call `exitErrorf`\. If there are no errors, notify the user of success\.

```
if err != nil {
    exitErrorf("Error occurred while waiting for bucket to be created, %v", bucket)
}

fmt.Printf("Bucket %q successfully created\n", bucket)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_create_bucket.go) on GitHub\.

## List Bucket Items<a name="s3-examples-bucket-ops-list-bucket-items"></a>

The [ListObjects](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.ListObjects) function lists the items in a bucket\.

The following example lists the items in the bucket with the name specified as a command line argument\.

Create the file *s3\_list\_objects\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    "fmt"
    "os"
)
```

Create a function we use to display errors and exit\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

The program requires one command line argument, the name of the bucket\.

```
if len(os.Args) != 2 {
    exitErrorf("Bucket name required\nUsage: %s bucket_name",
        os.Args[0])
}

bucket := os.Args[1]
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `~/.aws/credentials`, and create a new Amazon S3 service client\.

```
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Create S3 service client
svc := s3.New(sess)
```

Call [ListObjects](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.ListObjects), passing in the name of the bucket\. If an error occurs, call `exitErrorf`\. If no error occurs, loop through the items, printing the name, last modified date, size, and storage class of each item\.

```
resp, err := svc.ListObjectsV2(&s3.ListObjectsV2Input{Bucket: aws.String(bucket)})
if err != nil {
    exitErrorf("Unable to list items in bucket %q, %v", bucket, err)
}

for _, item := range resp.Contents {
    fmt.Println("Name:         ", *item.Key)
    fmt.Println("Last modified:", *item.LastModified)
    fmt.Println("Size:         ", *item.Size)
    fmt.Println("Storage class:", *item.StorageClass)
    fmt.Println("")
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_list_objects.go) on GitHub\.

## Upload a File to a Bucket<a name="s3-examples-bucket-ops-upload-file-to-bucket"></a>

The [Upload](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3manager/#Uploader.Upload) function uploads an object to a bucket\.

The following example uploads a file to a bucket with the names specified as command line arguments\.

Create the file *s3\_upload\_object\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3/s3manager"
    "fmt"
    "os"
)
```

Create a function we use to display errors and exit\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

Get the bucket and file name from the command line arguments, open the file, and defer the file closing until we are done with it\. If an error occurs, call `exitErrorF`\.

```
if len(os.Args) != 3 {
    exitErrorf("bucket and file name required\nUsage: %s bucket_name filename",
        os.Args[0])
}

bucket := os.Args[1]
filename := os.Args[2]

file, err := os.Open(filename)
if err != nil {
    exitErrorf("Unable to open file %q, %v", err)
}

defer file.Close()
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `~/.aws/credentials`, and create a `NewUploader` object\.

```
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Setup the S3 Upload Manager. Also see the SDK doc for the Upload Manager
// for more information on configuring part size, and concurrency.
//
// http://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3manager/#NewUploader
uploader := s3manager.NewUploader(sess)
```

Upload the file to the bucket\. If an error occurs, call `exitErrorF`\. Otherwise, notify the user that the upload succeeded\.

```
_, err = uploader.Upload(&s3manager.UploadInput{
    Bucket: aws.String(bucket),
    Key: aws.String(filename),
    Body: file,
})
if err != nil {
    // Print the error and exit.
    exitErrorf("Unable to upload %q to %q, %v", filename, bucket, err)
}

fmt.Printf("Successfully uploaded %q to %q\n", filename, bucket)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_upload_object.go) on GitHub\.

## Download a File from a Bucket<a name="s3-examples-bucket-ops-download-file-from-bucket"></a>

The [Download](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3manager/#Downloader.Download) function downloads an object from a bucket\.

The following example downloads an item from a bucket with the names specified as command line arguments\.

Create the file *s3\_download\_object\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    "github.com/aws/aws-sdk-go/service/s3/s3manager"
    
    "fmt"
    "os"
)
```

Create a function we use to display errors and exit\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

Get the bucket and file name from the command line arguments\. If there aren’t two arguments, call `exitErrorf`\. Otherwise, create the file and defer file closing until we are done downloading\. If an error occurs while creating the file, call `exitErrorf`\.

```
if len(os.Args) != 3 {
    exitErrorf("Bucket and item names required\nUsage: %s bucket_name item_name",
        os.Args[0])
}

bucket := os.Args[1]
item := os.Args[2]
```

Initialize the session in us\-west\-2 that the SDK uses to load credentials from the shared credentials file `~/.aws/credentials`, and create a `NewDownloader` object\.

```
sess, _ := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

downloader := s3manager.NewDownloader(sess)
```

Download the item from the bucket\. If an error occurs, call `exitErrorf`\. Otherwise, notify the user that the download succeeded\.

```
numBytes, err := downloader.Download(file,
    &s3.GetObjectInput{
        Bucket: aws.String(bucket),
        Key:    aws.String(item),
    })
if err != nil {
    exitErrorf("Unable to download item %q, %v", item, err)
}

fmt.Println("Downloaded", file.Name(), numBytes, "bytes")
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_download_object.go) on GitHub\.

## Copy an Item from one Bucket to Another<a name="s3-examples-bucket-ops-copy-bucket-item"></a>

The [CopyObject](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.CopyObject) function copies an object from one bucket to another\.

The following example copies an item from one bucket to another with the names specified as command line arguments\.

Create the file *s3\_copy\_object\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    "fmt"
    "net/url"
    "os"
)
```

Create a function we use to display errors and exit\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

Get the names of the bucket containing the item, the item to copy, and the name of the bucket to which the item is copied\. If there aren’t four command line arguments, call `exitErrorf`\.

```
if len(os.Args) != 4 {
    exitErrorf("Bucket, item, and other bucket names required\nUsage: go run s3_copy_object bucket item other-bucket")
}

bucket := os.Args[1]
item := os.Args[2]
other := os.Args[3]

source := bucket + "/" + item
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `~/.aws/credentials`, and create a new Amazon S3 service client\.

```
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Create S3 service client
svc := s3.New(sess)
```

Call [CopyObject](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.CopyObject), with the names of the bucket containing the item, the item to copy, and the name of the bucket to which the item is copied\. If an error occurs, call `exitErrorf`\. If no error occurs, wait for the item to be copied\.

```
// Copy the item
_, err = svc.CopyObject(&s3.CopyObjectInput{Bucket: aws.String(other),
    CopySource: aws.String(url.PathEscape(source)), Key: aws.String(item)})
if err != nil {
    exitErrorf("Unable to copy item from bucket %q to bucket %q, %v", bucket, other, err)
}
```

If the `WaitUntilObjectExists` call returns an error, call `exitErrorf`\. Otherwise, notify the user that the copy succeeded\.

```
// Wait to see if the item got copied
err = svc.WaitUntilObjectExists(&s3.HeadObjectInput{Bucket: aws.String(other), Key: aws.String(item)})
if err != nil {
    exitErrorf("Error occurred while waiting for item %q to be copied to bucket %q, %v", bucket, item, other, err)
}

fmt.Printf("Item %q successfully copied from bucket %q to bucket %q\n", item, bucket, other)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_copy_object.go) on GitHub\.

## Delete an Item in a Bucket<a name="s3-examples-bucket-ops-delete-bucket-item"></a>

The [DeleteObject](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.DeleteObject) function deletes an object from a bucket\.

The following example deletes an item from a bucket with the names specified as command line arguments\.

Create the file *s3\_delete\_object\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    "fmt"
    "os"
)
```

Create a function we use to display errors and exit\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

Get the name of the bucket and object to delete\.

```
if len(os.Args) != 3 {
    exitErrorf("Bucket and object name required\nUsage: %s bucket_name object_name",
        os.Args[0])
}

bucket := os.Args[1]
obj := os.Args[2]
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `~/.aws/credentials`, and create a new Amazon S3 service client\.

```
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Create S3 service client
svc := s3.New(sess)
```

Call [DeleteObject](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.DeleteObject), passing in the names of the bucket and object to delete\. If an error occurs, call `exitErrorf`\. If no error occurs, wait until the object is deleted\.

```
_, err = svc.DeleteObject(&s3.DeleteObjectInput{Bucket: aws.String(bucket), Key: aws.String(obj)})
if err != nil {
    exitErrorf("Unable to delete object %q from bucket %q, %v", obj, bucket, err)
}

err = svc.WaitUntilObjectNotExists(&s3.HeadObjectInput{
    Bucket: aws.String(bucket),
    Key:    aws.String(obj),
})
```

If `WaitUntilObjectNotExists` returns an error, call `exitErrorf`\. Otherwise, inform the user that the object was successfully deleted\.

```
fmt.Printf("Object %q successfully deleted\n", obj)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_delete_object.go) on GitHub\.

## Delete All the Items in a Bucket<a name="s3-examples-bucket-ops-delete-all-bucket-items"></a>

The [DeleteObjects](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.DeleteObjects) function deletes objects from a bucket\.

The following example deletes all the items from a bucket with the bucket name specified as a command line argument\.

Create the file *s3\_delete\_objects\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    "github.com/aws/aws-sdk-go/service/s3/s3manager"
    
    "fmt"
    "os"
)
```

Create a function we use to display errors and exit\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

Get the name of the bucket\.

```
if len(os.Args) != 2 {
    exitErrorf("Bucket name required\nUsage: %s BUCKET", os.Args[0])
}

bucket := os.Args[1]
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `~/.aws/credentials`, and create a new Amazon S3 service client\.

```
sess, _ := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)
svc := s3.New(sess)
```

Create a list iterator to iterate through the list of bucket objects, deleting each object\. If an error occurs, call `exitErrorf`\.

```
iter := s3manager.NewDeleteListIterator(svc, &s3.ListObjectsInput{
    Bucket: aws.String(bucket),
})

if err := s3manager.NewBatchDeleteWithClient(svc).Delete(aws.BackgroundContext(), iter); err != nil {
    exitErrorf("Unable to delete objects from bucket %q, %v", bucket, err)
}
```

Once all of the items in the bucket have been deleted, inform the user that the objects were deleted\.

```
fmt.Printf("Deleted object(s) from bucket: %s", bucket)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_delete_objects.go) on GitHub\.

## Restore a Bucket Item<a name="s3-examples-bucket-ops-restore-bucket-item"></a>

The [RestoreObject](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.RestoreObject) function restores an item in a bucket\.

The following example restores the items in a bucket with the names specified as command line arguments\.

Create the file *s3\_restore\_object\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    "fmt"
    "os"
)
```

Create a function we use to display errors and exit\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

The program requires two arguments, the names of the bucket and object to restore\.

```
if len(os.Args) != 3 {
    exitErrorf("Bucket name and object name required\nUsage: %s bucket_name object_name",
        os.Args[0])
}

bucket := os.Args[1]
obj := os.Args[2]
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `~/.aws/credentials`, and create a new Amazon S3 service client\.

```
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Create S3 service client
svc := s3.New(sess)
```

Call [RestoreObject](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.RestoreObject), passing in the bucket and object names and the number of days to temporarily restore\. If an error occurs, call `exitErrorf`\. Otherwise, inform the user that the bucket should be restored in the next four hours or so\.

```
_, err = svc.RestoreObject(&s3.RestoreObjectInput{Bucket: aws.String(bucket), Key: aws.String(obj), RestoreRequest: &s3.RestoreRequest{Days: aws.Int64(30)}})
if err != nil {
    exitErrorf("Could not restore %s in bucket %s, %v", obj, bucket, err)
}

fmt.Printf("%q should be restored to %q in about 4 hours\n", obj, bucket)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_restore_object.go) on GitHub\.

## Delete a Bucket<a name="s3-examples-bucket-ops-delete-bucket"></a>

The [DeleteBucket](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.DeleteBucket) function deletes a bucket\.

The following example deletes the bucket with the name specified as a command line argument\.

Create the file *s3\_delete\_bucket\.go*\. Import the following Go and AWS SDK for Go packages\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    "fmt"
    "os"
)
```

Create a function we use to display errors and exit\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

The program requires one argument, the name of the bucket to delete\. If the argument is not supplied, call `exitErrorf`\.

```
if len(os.Args) != 2 {
    exitErrorf("bucket name required\nUsage: %s bucket_name", os.Args[0])
}

bucket := os.Args[1]
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `~/.aws/credentials`, and create a new S3 service client\.

```
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Create S3 service client
svc := s3.New(sess)
```

Call [DeleteBucket](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.DeleteBucket), passing in the bucket name\. If an error occurs, call `exitErrorf`\. If there are no errors, wait for a notification that the bucket was deleted\.

```
_, err = svc.DeleteBucket(&s3.DeleteBucketInput{
    Bucket: aws.String(bucket),
})
if err != nil {
    exitErrorf("Unable to delete bucket %q, %v", bucket, err)
}

// Wait until bucket is deleted before finishing
fmt.Printf("Waiting for bucket %q to be deleted...\n", bucket)

err = svc.WaitUntilBucketNotExists(&s3.HeadBucketInput{
    Bucket: aws.String(bucket),
})
```

If `WaitUntilBucketNotExists` returns an error, call `exitErrorf`\. Otherwise, inform the user that the bucket was successfully deleted\.

```
if err != nil {
    exitErrorf("Error occurred while waiting for bucket to be deleted, %v", bucket)
}

fmt.Printf("Bucket %q successfully deleted\n", bucket)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_delete_bucket.go) on GitHub\.