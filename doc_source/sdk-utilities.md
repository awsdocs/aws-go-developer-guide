# Using the AWS SDK for Go Utilities<a name="sdk-utilities"></a>

The AWS SDK for Go includes the following utilities to help you more easily use AWS services\. Find the SDK utilities in their related AWS service package\.

## Amazon CloudFront URL Signer<a name="cf-url-signer"></a>

The Amazon CloudFront URL signer simplifies the process of creating signed URLs\. A signed URL includes information, such as an expiration date and time, that enables you to control access to your content\. Signed URLs are useful when you want to distribute content through the internet, but want to restrict access to certain users \(for example, to users who have paid a fee\)\.

To sign a URL, create a `URLSigner` instance with your CloudFront key pair ID and the associated private key\. Then call the `Sign` or `SignWithPolicy` method and include the URL to sign\. For more information about Amazon CloudFront key pairs, see [Creating CloudFront Key Pairs for Your Trusted Signers](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-trusted-signers.html#private-content-creating-cloudfront-key-pairs) in the Amazon CloudFront Developer Guide\.

The following example creates a signed URL that’s valid for one hour after it is created\.

```
signer := sign.NewURLSigner(keyID, privKey)

signedURL, err := signer.Sign(rawURL, time.Now().Add(1*time.Hour))
if err != nil {
    log.Fatalf("Failed to sign url, err: %s\n", err.Error())
    return
}
```

For more information about the signing utility, see the [sign](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudfront/sign/) package in the AWS SDK for Go API Reference\.

## Amazon DynamoDB Attributes Converter<a name="ddb-attributes-converter"></a>

The attributes converter simplifies converting Amazon DynamoDB attribute values to and from concrete Go types\. Conversions make it easy to work with attribute values in Go and to write values to Amazon DynamoDB tables\. For example, you can create records in Go and then use the converter when you want to write those records as attribute values to a DynamoDB table\.

The following example converts a structure to an Amazon DynamoDB`AttributeValues` map and then puts the data to the `exampleTable`\.

```
type Record struct {
    MyField string
    Letters []string
    A2Num   map[string]int
}
r := Record{
    MyField: "dynamodbattribute.ConvertToX example",
    Letters: []string{"a", "b", "c", "d"},
    A2Num:   map[string]int{"a": 1, "b": 2, "c": 3},
}

//...

svc := dynamodb.New(session.New(&aws.Config{Region: aws.String("us-west-2")}))
item, err := dynamodbattribute.ConvertToMap(r)
if err != nil {
    fmt.Println("Failed to convert", err)
    return
}
result, err := svc.PutItem(&dynamodb.PutItemInput{
    Item:      item,
    TableName: aws.String("exampleTable"),
})
fmt.Println("Item put to dynamodb", result, err)
```

For more information about the converter utility, see the [dynamodbattribute](https://docs.aws.amazon.com/sdk-for-go/api/service/dynamodb/dynamodbattribute/) package in the AWS SDK for Go API Reference\.

## Amazon Elastic Compute Cloud Metadata<a name="ec2-metadata"></a>

 `EC2Metadata` is a client that interacts with the Amazon EC2 metadata service\. The client can help you easily retrieve information about instances on which your applications run, such as its region or local IP address\. Typically, you must create and submit HTTP requests to retrieve instance metadata\. Instead, create an `EC2Metadata` service client\.

```
c := ec2metadata.New(session.New())
```

Then use the service client to retrieve information from a metadata category like `local-ipv4` \(the private IP address of the instance\)\.

```
localip, err := c.GetMetadata("local-ipv4")
if err != nil {
    log.Printf("Unable to retrieve the private IP address from the EC2 instance: %s\n", err)
    return
}
```

For a list of all metadata categories, see [Instance Metadata Categories](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html#instancedata-data-categories) in the Amazon EC2 User Guide for Linux Instances\.

### Retrieving an Instance’s Region<a name="retrieving-an-instances-region"></a>

There’s no instance metadata category that returns only the region of an instance\. Instead, use the included `Region` method to easily return an instance’s region\.

```
region, err := ec2metadata.New(session.New()).Region()
if err != nil {
    log.Printf("Unable to retrieve the region from the EC2 instance %v\n", err)
}
```

For more information about the EC2 metadata utility, see the [ec2metadata](https://docs.aws.amazon.com/sdk-for-go/api/aws/ec2metadata/) package in the AWS SDK for Go API Reference\.

## Amazon S3 Transfer Managers<a name="s3-transfer-managers"></a>

The Amazon Simple Storage Service upload and download managers can break up large objects so they can be transferred in multiple parts, in parallel\. This makes it easy to resume interrupted transfers\.

### Upload Manager<a name="upload-manager"></a>

The Amazon Simple Storage Service upload manager determines if a file can be split into smaller parts and uploaded in parallel\. You can customize the number of parallel uploads and the size of the uploaded parts\.

#### Example: Uploading a File<a name="uploading"></a>

The following example uses the Amazon S3`Uploader` to upload a file\. Using `Uploader` is similar to the `s3.PutObject()` operation\.

```
mySession, _ := session.NewSession()
uploader := s3manager.NewUploader(mySession)
result, err := uploader.Upload(&s3manager.UploadInput{
    Bucket: &uploadBucket,
    Key:    &uploadFileKey,
    Body:   uploadFile,
})
```

#### Configuration Options<a name="configuration-options"></a>

When you instantiate an `Uploader` instance, you can specify several configuration options \(`UploadOptions`\) to customize how objects are uploaded:
+  `PartSize`– Specifies the buffer size, in bytes, of each part to upload\. The minimum size per part is 5 MB\.
+  `Concurrency`– Specifies the number of parts to upload in parallel\.
+  `LeavePartsOnError`– Indicates whether to leave successfully uploaded parts in Amazon S3\.

Tweak the `PartSize` and `Concurrency` configuration values to find the optimal configuration\. For example, systems with high\-bandwidth connections can send bigger parts and more uploads in parallel\.

For more information about `Uploader` and its configurations, see the [s3manager](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3manager/#Uploader) package in the AWS SDK for Go API Reference\.

#### UploadInput Body Field \(io\.ReadSeeker vs\. io\.Reader\)<a name="uploadinput-body-field"></a>

The `Body` field of the `s3manager.UploadInput` struct is an `io.Reader` type\. However, the field also satisfies the `io.ReadSeeker` interface\.

For `io.ReadSeeker` types, the `Uploader` doesn’t buffer the body contents before sending it to Amazon S3\. `Uploader` calculates the expected number of parts before uploading the file to Amazon S3\. If the current value of `PartSize` requires more than 10,000 parts to upload the file, `Uploader` increases the part size value so that fewer parts are required\.

For `io.Reader` types, the bytes of the reader must buffer each part in memory before the part is uploaded\. When you increase the `PartSize` or `Concurrency` value, the required memory \(RAM\) for the `Uploader` increases significantly\. The required memory is approximately *``PartSize``* \* *``Concurrency``*\. For example, if you specify 100 MB for `PartSize` and 10 for `Concurrency`, the required memory will be at least 1 GB\.

Because an `io.Reader` type cannot determine its size before reading its bytes, `Uploader` cannot calculate how many parts must be uploaded\. Consequently, `Uploader` can reach the Amazon S3 upload limit of 10,000 parts for large files if you set the `PartSize` too low\. If you try to upload more than 10,000 parts, the upload stops and returns an error\.

#### Handling Partial Uploads<a name="handling-partial-uploads"></a>

If an upload to Amazon S3 fails, by default, `Uploader` uses the Amazon S3`AbortMultipartUpload` operation to remove the uploaded parts\. This functionality ensures that failed uploads do not consume Amazon S3 storage\.

You can set `LeavePartsOnError` to true so that the `Uploader` doesn’t delete successfully uploaded parts\. This is useful for resuming partially completed uploads\. To operate on uploaded parts, you must get the `UploadID` of the failed upload\. The following example demonstrates how to use the `s3manager.MultiUploadFailure` message to get the `UploadID`\.

```
u := s3manager.NewUploader(session.New())
output, err := u.upload(input)
if err != nil {
    if multierr, ok := err.(s3manager.MultiUploadFailure); ok {
        // Process error and its associated uploadID
        fmt.Println("Error:", multierr.Code(), multierr.Message(), multierr.UploadID())
    } else {
        // Process error generically
        fmt.Println("Error:", err.Error())
    }
}
```

#### Example: Upload a Folder to Amazon S3<a name="example-upload-a-folder-to-s3"></a>

The following example uses the `path/filepath` package to recursively gather a list of files and upload them to the specified Amazon S3 bucket\. The keys of the Amazon S3 objects are prefixed with the file’s relative path\.

```
package main

import (
    "log"
    "os"
    "path/filepath"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3/s3manager"
)

var (
    localPath string
    bucket    string
    prefix    string
)

func init() {
    if len(os.Args) != 4 {
        log.Fatalln("Usage:", os.Args[0], "<local path> <bucket> <prefix>")
    }
    localPath = os.Args[1]
    bucket = os.Args[2]
    prefix = os.Args[3]
}

func main() {
    walker := make(fileWalk)
    go func() {
        // Gather the files to upload by walking the path recursively
        if err := filepath.Walk(localPath, walker.Walk); err != nil {
            log.Fatalln("Walk failed:", err)
        }
        close(walker)
    }()

    // For each file found walking, upload it to S3
    uploader := s3manager.NewUploader(session.New())
    for path := range walker {
        rel, err := filepath.Rel(localPath, path)
        if err != nil {
            log.Fatalln("Unable to get relative path:", path, err)
        }
        file, err := os.Open(path)
        if err != nil {
            log.Println("Failed opening file", path, err)
            continue
        }
        defer file.Close()
        result, err := uploader.Upload(&s3manager.UploadInput{
            Bucket: &bucket,
            Key:    aws.String(filepath.Join(prefix, rel)),
            Body:   file,
        })
        if err != nil {
            log.Fatalln("Failed to upload", path, err)
        }
        log.Println("Uploaded", path, result.Location)
    }
}

type fileWalk chan string

func (f fileWalk) Walk(path string, info os.FileInfo, err error) error {
    if err != nil {
        return err
    }
    if !info.IsDir() {
        f <- path
    }
    return nil
}
```

#### Example: Upload a File to Amazon S3 and Send its Location to Amazon SQS<a name="upload-file-to-s3"></a>

The following example uploads a file to an Amazon S3 bucket and then sends a notification message of the file’s location to an Amazon Simple Queue Service queue\.

```
package main

import (
    "log"
    "os"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3/s3manager"
    "github.com/aws/aws-sdk-go/service/sqs"
)

// Uploads a file to a specific bucket in S3 with the file name
// as the object's key. After it's uploaded, a message is sent
// to a queue.
func main() {
    if len(os.Args) != 4 {
        log.Fatalln("Usage:", os.Args[0], "<bucket> <queue> <file>")
    }

    file, err := os.Open(os.Args[3])
    if err != nil {
        log.Fatal("Open failed:", err)
    }
    defer file.Close()

    // Upload the file to S3 using the S3 Manager
    uploader := s3manager.NewUploader(session.New())
    uploadRes, err := uploader.Upload(&s3manager.UploadInput{
        Bucket: aws.String(os.Args[1]),
        Key:    aws.String(file.Name()),
        Body:   file,
    })
    if err != nil {
        log.Fatalln("Upload failed:", err)
    }

    // Get the URL of the queue that the message will be posted to
    svc := sqs.New(session.New())
    urlRes, err := svc.GetQueueUrl(&sqs.GetQueueUrlInput{
        QueueName: aws.String(os.Args[2]),
    })
    if err != nil {
        log.Fatalln("GetQueueURL failed:", err)
    }

    // Send the message to the queue
    _, err = svc.SendMessage(&sqs.SendMessageInput{
        MessageBody: &uploadRes.Location,
        QueueUrl:    urlRes.QueueUrl,
    })
    if err != nil {
        log.Fatalln("SendMessage failed:", err)
    }
}
```

### Download Manager<a name="download-manager"></a>

The Amazon S3 download manager determines if a file can be split into smaller parts and downloaded in parallel\. You can customize the number of parallel downloads and the size of the downloaded parts\.

#### Example: Download a File<a name="downloading"></a>

The following example uses the Amazon S3`Downloader` to download a file\. Using `Downloader` is similar to the `s3.GetObject()` operation\.

```
downloader := s3manager.NewDownloader(session.New())
numBytes, err := downloader.Download(downloadFile,
  &s3.GetObjectInput{
    Bucket: &downloadBucket,
    Key:    &downloadFileKey,
})
```

The `downloadFile` parameter is an `io.WriterAt` type\. The `WriterAt` interface enables the `Downloader` to write multiple parts of the file in parallel\.

#### Configuration Options<a name="id5"></a>

When you instantiate a `Downloader` instance, you can specify several configuration options \(`DownloadOptions`\) to customize how objects are downloaded:
+  `PartSize`– Specifies the buffer size, in bytes, of each part to download\. The minimum size per part is 5 MB\.
+  `Concurrency`– Specifies the number of parts to download in parallel\.

Tweak the `PartSize` and `Concurrency` configuration values to find the optimal configuration\. For example, systems with high\-bandwidth connections can receive bigger parts and more downloads in parallel\.

For more information about `Downloader` and its configurations, see the [s3manager](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3manager/#Downloader) package in the AWS SDK for Go API Reference\.

#### Example: Download All Objects in a Bucket<a name="download-all-objects-in-a-bucket"></a>

The following example uses pagination to gather a list of objects from an Amazon S3 bucket\. Then it downloads each object to a local file\.

```
package main

import (
    "fmt"
    "os"
    "path/filepath"

    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    "github.com/aws/aws-sdk-go/service/s3/s3manager"
)

var (
    Bucket         = "MyBucket" // Download from this bucket
    Prefix         = "logs/"    // Using this key prefix
    LocalDirectory = "s3logs"   // Into this directory
)

func main() {
    manager := s3manager.NewDownloader(session.New())
    d := downloader{bucket: Bucket, dir: LocalDirectory, Downloader: manager}

    client := s3.New(session.New())
    params := &s3.ListObjectsInput{Bucket: &Bucket, Prefix: &Prefix}
    client.ListObjectsPages(params, d.eachPage)
}

type downloader struct {
    *s3manager.Downloader
    bucket, dir string
}

func (d *downloader) eachPage(page *s3.ListObjectsOutput, more bool) bool {
    for _, obj := range page.Contents {
        d.downloadToFile(*obj.Key)
    }

    return true
}

func (d *downloader) downloadToFile(key string) {
    // Create the directories in the path
    file := filepath.Join(d.dir, key)
    if err := os.MkdirAll(filepath.Dir(file), 0775); err != nil {
        panic(err)
    }

    // Set up the local file
    fd, err := os.Create(file)
    if err != nil {
        panic(err)
    }
    defer fd.Close()

    // Download the file using the AWS SDK for Go
    fmt.Printf("Downloading s3://%s/%s to %s...\n", d.bucket, key, file)
    params := &s3.GetObjectInput{Bucket: &d.bucket, Key: &key}
    d.Download(fd, params)
}
```