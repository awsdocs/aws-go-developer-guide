# Re\-encrypting a Data Blob in AWS Key Management Service<a name="kms-example-re-encrypt-data"></a>

The following example uses the AWS SDK for Go[ReEncrypt](https://docs.aws.amazon.com/sdk-for-go/api/service/kms/#KMS.ReEncrypt) method, which implements the [ReEncrypt](https://docs.aws.amazon.com/kms/latest/APIReference/API_ReEncrypt.html) operation, to decrypt encrypted data and then immediately re\-encrypt data under a new customer master key \(CMK\)\. The operations are performed entirely on the server side within AWS KMS, so they never expose your plaintext outside of AWS KMS\. The example displays a readable version of the resulting re\-encrypted blob\.

```
import (
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/kms"

    "fmt"
    "os"
)

func main() {
    // Initialize a session that the SDK uses to load
    // credentials from the shared credentials file ~/.aws/credentials
    // and configuration from the shared configuration file ~/.aws/config.
    sess := session.Must(session.NewSessionWithOptions(session.Options{
        SharedConfigState: session.SharedConfigEnable,
    }))

    // Create KMS service client
    svc := kms.New(sess)

    // Encrypt data key
    //
    // Replace the fictitious key ARN with a valid key ID

    keyId := "arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-1234567890ab"

    // Encrypted data
    blob := []byte("1234567890")

    // Re-encrypt the data key
    result, err := svc.ReEncrypt(&kms.ReEncryptInput{CiphertextBlob: blob, DestinationKeyId: &keyId})

    if err != nil {
        fmt.Println("Got error re-encrypting data: ", err)
        os.Exit(1)
    }

    fmt.Println("Blob (base-64 byte array):")
    fmt.Println(result.CiphertextBlob)
```

Choose `Copy` to save the code locally\. See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/kms/kms_re_encrypt_data.go) on GitHub\.