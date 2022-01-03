# Encrypting Data with AWS Key Management Service<a name="kms-example-encrypt-data"></a>

The following example uses the AWS SDK for Go[Encrypt](https://docs.aws.amazon.com/sdk-for-go/api/service/kms/#KMS.Encrypt) method, which implements the [Encrypt](https://docs.aws.amazon.com/kms/latest/APIReference/API_Encrypt.html) operation, to encrypt the string “1234567890”\. The example displays a readable version of the resulting encrypted blob\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/kms"

    "fmt"
    "os"
)

func main() {
    // Initialize a session in us-west-2 that the SDK will use to load
    // credentials from the shared credentials file ~/.aws/credentials.
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create KMS service client
    svc := kms.New(sess)

    // Encrypt data key
    //
    // Replace the fictitious key ARN with a valid key ID

    keyId := "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab"

    text := "1234567890"

    // Encrypt the data
    result, err := svc.Encrypt(&kms.EncryptInput{
        KeyId: aws.String(keyId),
        Plaintext: []byte(text),
    })

    if err != nil {
        fmt.Println("Got error encrypting data: ", err)
        os.Exit(1)
    }

    fmt.Println("Blob (base-64 byte array):")
    fmt.Println(result.CiphertextBlob)
}
```

Choose `Copy` to save the code locally\. See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/kms/kms_encrypt_data.go) on GitHub\.