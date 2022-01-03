# Decrypting a Data Blob in AWS Key Management Service<a name="kms-example-decrypt-blob"></a>

The following example uses the AWS SDK for Go[Decrypt](https://docs.aws.amazon.com/sdk-for-go/api/service/kms/#KMS.Decrypt) method, which implements the [Decrypt](https://docs.aws.amazon.com/kms/latest/APIReference/API_Decrypt.html) operation, to decrypt the provided string and emits the result\.

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

    // Encrypted data
    blob := []byte("1234567890")

    // Decrypt the data
    result, err := svc.Decrypt(&kms.DecryptInput{CiphertextBlob: blob})

    if err != nil {
        fmt.Println("Got error decrypting data: ", err)
        os.Exit(1)
    }

    blob_string := string(result.Plaintext)

    fmt.Println(blob_string)
```

Choose `Copy` to save the code locally\. See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/kms/kms_decrypt_data.go) on GitHub\.