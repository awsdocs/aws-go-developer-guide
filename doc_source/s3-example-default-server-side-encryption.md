# Setting Default Server\-Side Encryption for an Amazon S3 Bucket<a name="s3-example-default-server-side-encryption"></a>

The following example uses the [PutBucketEncryption](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutBucketEncryption) method to enable KMS server\-side encryption on any objects added to `myBucket`\.

The only exception is if the user configures their request to explicitly use server\-side encryption\. In that case, the specified encryption takes precedence\.

Choose `Copy` to save the code locally\.

Create the file *set\_default\_encryption\.go*\.

Import the required packages\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"

    "fmt"
    "os"
)
```

Get the KMS key from the command line, where `key` is a KMS key ID as created in the [Creating a CMK in AWS Key Management Service](kms-example-create-key.md) example, and set the bucket name\.

```
if len(os.Args) != 2 {
    fmt.Println("You must supply a key")
    os.Exit(1)
}

key := os.Args[1]
bucket := "myBucket"
```

Create a session and Amazon S3 client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

svc := s3.New(sess)
```

Create the input for and call `PutBucketEncryption`, and display a success message\.

```
defEnc := &s3.ServerSideEncryptionByDefault{KMSMasterKeyID: aws.String(key), SSEAlgorithm: aws.String(s3.ServerSideEncryptionAwsKms)}
rule := &s3.ServerSideEncryptionRule{ApplyServerSideEncryptionByDefault: defEnc}
rules := []*s3.ServerSideEncryptionRule{rule}
serverConfig := &s3.ServerSideEncryptionConfiguration{Rules: rules}
input := &s3.PutBucketEncryptionInput{Bucket: aws.String(bucket), ServerSideEncryptionConfiguration: serverConfig}
_, err := svc.PutBucketEncryption(input)
fmt.Println("Bucket " + bucket + " now has KMS encryption by default")
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_set_default_encryption.go) on GitHub\.