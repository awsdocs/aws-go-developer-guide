# Creating a CMK in AWS Key Management Service<a name="kms-example-create-key"></a>

The following example uses the AWS SDK for Go[CreateKey](https://docs.aws.amazon.com/sdk-for-go/api/service/kms/#KMS.CreateKey) method, which implements the [CreateKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateKey.html) operation, to create a customer master key \(CMK\)\. Since the example only encrypts a small amount of data, a CMK is fine for our purposes\. For larger amounts of data, use the CMK to encrypt a data encryption key \(DEK\)\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/kms"

    "fmt"
    "os"
)

// Create an AWS KMS key (KMS key)
// Since we are only encrypting small amounts of data (4 KiB or less) directly,
// a KMS key is fine for our purposes.
// For larger amounts of data,
// use the KMS key to encrypt a data encryption key (DEK).

func main() {
    // Initialize a session in us-west-2 that the SDK will use to load
    // credentials from the shared credentials file ~/.aws/credentials.
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create KMS service client
    svc := kms.New(sess)

    // Create the key
    result, err := svc.CreateKey(&kms.CreateKeyInput{
        Tags: []*kms.Tag{
            {
                TagKey:   aws.String("CreatedBy"),
                TagValue: aws.String("ExampleUser"),
            },
        },
    })

    if err != nil {
        fmt.Println("Got error creating key: ", err)
        os.Exit(1)
    }

    fmt.Println(*result.KeyMetadata.KeyId)
}
```

Choose `Copy` to save the code locally\. See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/kms/kms_create_key.go) on GitHub\.