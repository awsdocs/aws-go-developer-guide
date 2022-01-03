# Encrypting an Amazon S3 Bucket Object on the Server Using AWS KMS<a name="s3-example-server-side-encryption-with-kms"></a>

The following example uses the [PutObject](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutObject) method to add the object `myItem` to the bucket `myBucket` with server\-side encryption set to AWS KMS\.

Note that this differs from [Setting Default Server\-Side Encryption for an Amazon S3 Bucket](s3-example-default-server-side-encryption.md), is in that case, the objects are encrypted without you having to explicitly perform the operation\.

Choose `Copy` to save the code locally\.

Create the file *encrypt\_object\_on\_server\.go*\.

Add the required packages\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"

    "fmt"
    "os"
    "strings"
)
```

Get the KMS key from the command line, where `key` is a KMS key ID as created in the [Creating a CMK in AWS Key Management Service](kms-example-create-key.md) example, and set the bucket and object names\.

```
if len(os.Args) != 2 {
    fmt.Println("You must supply a key")
    os.Exit(1)
}

key := os.Args[1]
bucket := "myBucket"
object := "myItem"
```

Create a session and Amazon S3 client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

svc := s3.New(sess)
```

Create input for and call `put_object`\. Notice that the `server_side_encryption` property is set to `aws:kms`, indicating that Amazon S3 encrypts the object using AWS KMS, and display a success message to the user\.

```
input := &s3.PutObjectInput{
    Body:                 strings.NewReader(object),
    Bucket:               aws.String(bucket),
    Key:                  aws.String(object),
    ServerSideEncryption: aws.String("aws:kms"),
    SSEKMSKeyId:          aws.String(key),
}

_, err := svc.PutObject(input)
fmt.Println("Added object " + object + " to bucket " + bucket + " with AWS KMS encryption")
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_encrypt_on_server_with_kms.go) on GitHub\.