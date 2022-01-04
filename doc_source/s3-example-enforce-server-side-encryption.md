# Requiring Encryption on the Server to Upload Amazon S3 Bucket Objects<a name="s3-example-enforce-server-side-encryption"></a>

The following example uses the [PutBucketPolicy](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutBucketPolicy) method to require that objects uploaded to an Amazon S3 bucket have Amazon S3 encrypt the object with an AWS KMS key\. Attempts to upload an object without specifying that Amazon S3 encrypt the object with an AWS KMS key raise an `Aws::S3::Errors::AccessDenied` exception\.

Avoid using this configuration option if you use default server\-side encryption as described in [Setting Default Server\-Side Encryption for an Amazon S3 Bucket](s3-example-default-server-side-encryption.md) as they could conflict and result in unexpected results\.

Choose `Copy` to save the code locally\.

Create the file *require\_server\_encryption\.go*\.

Import the required packages\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"

    "fmt"
    "os"
    "encoding/json"
)
```

Set the name of the bucket, create a session, and create an Amazon S3 client\.

```
bucket := "myBucket"
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

svc := s3.New(sess)
```

Create an Amazon S3 policy that requires server\-side KMS encryption on objects uploaded to the bucket\.

```
PolicyDoc := map[string]interface{}{
    "Version": "2012-10-17",
    "Statement": []map[string]interface{}{
        {
            "Sid": "DenyIncorrectEncryptionHeader",
            "Effect": "Deny",
            "Principal": "*",
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::" + bucket + "/*",
            "Condition": map[string]interface{}{
                "StringNotEquals": map[string]interface{}{
                    "s3:x-amz-server-side-encryption": "aws:kms",
                },
            },
        },
        {
            "Sid": "DenyUnEncryptedObjectUploads",
            "Effect": "Deny",
            "Principal": "*",
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::" + bucket + "/*",
            "Condition": map[string]interface{}{
                "Null": map[string]interface{}{
                    "s3:x-amz-server-side-encryption": "true",
                },
            },
        },
    },
}
```

Convert the policy into JSON, create the input for and call `PutBucketPolicy`, apply the policy to the bucket, and print a success message\.

```
policy, err := json.Marshal(PolicyDoc)
input := &s3.PutBucketPolicyInput{
    Bucket: aws.String(bucket),
    Policy: aws.String(string(policy)),
}
_, err = svc.PutBucketPolicy(input)
fmt.Println("Set policy for " + bucket)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_require_server_encryption.go) on GitHub\.