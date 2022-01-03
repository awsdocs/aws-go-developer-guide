# Amazon S3 Glacier Examples Using the AWS SDK for Go<a name="using-glacier-with-go-sdk"></a>

Amazon S3 Glacier is a secure, durable, and extremely low\-cost cloud storage service for data archiving and long\-term backup\. The AWS SDK for Go examples can integrate Amazon S3 Glacier into your applications\. The examples assume you have already set up and configured the SDK \(that is, you’ve imported all required packages and set your credentials and region\)\. For more information, see [Getting Started with the AWS SDK for Go](setting-up.md) and [Configuring the AWS SDK for Go](configuring-sdk.md)\.

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/glacier) repository on GitHub\.

## The Scenario<a name="the-scenario"></a>

Amazon S3 Glacier is a secure cloud storage service for data archiving and long\-term backup\. The service is optimized for infrequently accessed data where a retrieval time of several hours is suitable\. These examples show you how to create a vault and upload an archive with Go\. The methods used include:
+  [CreateVault](https://docs.aws.amazon.com/sdk-for-go/api/service/glacier/#Glacier.CreateVault) 
+  [UploadArchive](https://docs.aws.amazon.com/sdk-for-go/api/service/glacier/#Glacier.UploadArchive) 

## Prerequisites<a name="create-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with the Amazon S3 Glacier data model\. To learn more, see [Amazon Glacier Data Model](https://docs.aws.amazon.com/amazonglacier/latest/dev/amazon-glacier-data-model.html) in the Amazon S3 Glacier Developer Guide\.

## Create a Vault<a name="create-a-vault"></a>

The following example uses the Amazon S3 Glacier[CreateVault](https://docs.aws.amazon.com/sdk-for-go/api/service/glacier/#Glacier.CreateVault) operation to create a vault named `YOUR_VAULT_NAME`\.

```
import (
    "log"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/glacier"
)

func main() {
    // Initialize a session that the SDK uses to load
    // credentials from the shared credentials file ~/.aws/credentials
    // and configuration from the shared configuration file ~/.aws/config.
    sess := session.Must(session.NewSessionWithOptions(session.Options{
        SharedConfigState: session.SharedConfigEnable,
    }))

    // Create Glacier client in default region
    svc := glacier.New(sess)

    // start snippet
    _, err := svc.CreateVault(&glacier.CreateVaultInput{
        VaultName: aws.String("YOUR_VAULT_NAME"),
    })
    if err != nil {
        log.Println(err)
        return
    }

    log.Println("Created vault!")
    // end snippet
}
```

## Upload an Archive<a name="upload-an-archive"></a>

The following example assumes you have a vault named `YOUR_VAULT_NAME`\. It uses the Amazon S3 Glacier[UploadArchive](https://docs.aws.amazon.com/sdk-for-go/api/service/glacier/#Glacier.UploadArchive) operation to upload a single reader object as an entire archive\. The AWS SDK for Go automatically computes the tree hash checksum for the data to be uploaded\.

```
import (
    "bytes"
    "log"

    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/glacier"
)

func main() {
    // Initialize a session that the SDK uses to load
    // credentials from the shared credentials file ~/.aws/credentials
    // and configuration from the shared configuration file ~/.aws/config.
    sess := session.Must(session.NewSessionWithOptions(session.Options{
        SharedConfigState: session.SharedConfigEnable,
    }))

    // Create Glacier client in default region
    svc := glacier.New(sess)

    // start snippet
    vaultName := "YOUR_VAULT_NAME"

    result, err := svc.UploadArchive(&glacier.UploadArchiveInput{
        VaultName: &vaultName,
        Body:      bytes.NewReader(make([]byte, 2*1024*1024)), // 2 MB buffer
    })
    if err != nil {
        log.Println("Error uploading archive.", err)
        return
    }

    log.Println("Uploaded to archive", *result.ArchiveId)
    // end snippet
}
```