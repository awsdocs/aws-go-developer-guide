# Amazon S3 Encryption Client Migration<a name="s3-encryption-migration"></a>

This topic shows how to migrate your applications from Version 1 \(V1\) of the Amazon Simple Storage Service \(Amazon S3\) encryption client to Version 2 \(V2\) and ensure application availability throughout the migration process\.

## Migration Overview<a name="migration-overview"></a>

This migration happens in two phases:

1\. **Update existing clients to read new formats\.** First, deploy an updated version of the AWS SDK for Go to your application\. This allows existing V1 encryption clients to decrypt objects written by the new V2 clients\. If your application uses multiple AWS SDKs, you must upgrade each SDK separately\.

2\. **Migrate encryption and decryption clients to V2\.** Once all of your V1 encryption clients can read new formats, you can migrate your existing encryption and decryption clients to their respective V2 versions\.

## Update Existing Clients to Read New Formats<a name="update-existing-clients-to-read-new-formats"></a>

The V2 encryption client uses encryption algorithms that older versions of the client do not support\. The first step in the migration is to update your V1 decryption clients to SDK version v1\.34\.0 or later\. After completing this step, your application’s V1 clients will be able to decrypt objects encrypted by V2 encryption clients\.

### Update Applications That Use Go Modules<a name="update-applications-that-use-go-modules"></a>

If your applications use the Go module dependency system introduced in Go 1\.11, you must take the following steps to update your application to the latest release of the AWS SDK for Go\.

First, update your SDK module dependency\.

```
$ go get github.com/aws/aws-sdk-go@latest
```

Next, validate your dependency has correctly updated to the required minimum version\.

```
$ go list -m github.com/aws/aws-sdk-go
github.com/aws/aws-sdk-go v1.34.0
```

After you upgrade and validate your dependencies, deploy the application to your fleet\. Once the rollout is complete, you can migrate your V1 encryption and decryption clients to V2\.

### Updating Applications That Use GOPATH<a name="updating-applications-that-use-gopath"></a>

If your application uses `GOPATH` to manage its dependencies, you must take the following steps to update and verify that your application is using the minimum SDK release or later\.

First, update your SDK `GOPATH` source code\.

```
$ go get -u github.com/aws/aws-sdk-go
```

Next, ensure the SDK source path to the release \(v1\.34\.0\) or later \([https://github\.com/aws/aws\-sdk\-go/releases](https://github.com/aws/aws-sdk-go/releases)\)\.

```
$ cd $GOPATH/src/github.com/aws/aws-sdk-go
$ git fetch
$ go checkout v1.34.0
```

After you upgrade your dependencies and verify, deploy the application to your fleet\. Once the rollout is complete, you can migrate your V1 encryption and decryption clients to V2\.

## Migrate Encryption and Decryption Clients to V2<a name="migrate-encryption-and-decryption-clients-to-v2"></a>

After updating your existing clients to read the new encryption formats, you can now proceed with safely updating your applications to the V2 encryption and decryption clients\. The next series of steps will guide you through successfully migrating your code from V1 to V2\.

### Migrate Cipher Data Generators<a name="migrate-cipher-data-generators"></a>

Applications that use [NewKMSKeyGenerator](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#NewKMSKeyGenerator) or [NewKMSKeyGeneratorWithMatDesc](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#NewKMSKeyGeneratorWithMatDesc) functions for constructing a [CipherDataGenerator](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#CipherDataGenerator) need to migrate their usage to [NewKMSContextKeyGenerator](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#NewKMSContextKeyGenerator)\. This migration is required because support for the former `CipherDataGenerators` have been deprecated\. Attempting to construct a V2 client using the old methods will result in an error during client construction\.

 **Example: Migrate NewKMSKeyGenerator** 

 *Pre\-migration* 

```
sess := session.Must(session.NewSession())
kmsClient := kms.New(sess)
cmkID := "1234abcd-12ab-34cd-56ef-1234567890ab"

cipherDataGenerator := s3crypto.NewKMSKeyGenerator(kmsClient, cmkID)
```

 *Post\-migration* 

```
sess := session.Must(session.NewSession())
kmsClient := kms.New(sess)
cmkID := "1234abcd-12ab-34cd-56ef-1234567890ab"
var matDesc s3crypto.MaterialDescription

// changed NewKMSKeyGenerator to NewKMSContextKeyGenerator
cipherDataGenerator := s3crypto.NewKMSContextKeyGenerator(kmsClient, cmkID, matDesc)
```

 **Example: Migrate NewKMSKeyGeneratorWithMatDesc** 

 *Pre\-migration* 

```
sess := session.Must(session.NewSession())
kmsClient := kms.New(sess)
cmkID := "1234abcd-12ab-34cd-56ef-1234567890ab"
matDesc := s3crypto.MaterialDescription{
    "custom-key": aws.String("custom value"),
}

cipherDataGenerator := s3crypto.NewKMSKeyGeneratorWithMatDesc(kmsClient, cmkID, matDesc)
```

 *Post\-migration* 

```
sess := session.Must(session.NewSession())
kmsClient := kms.New(sess)
cmkID := "1234abcd-12ab-34cd-56ef-1234567890ab"
matDesc := s3crypto.MaterialDescription{
    "custom-key": aws.String("custom value"),
}

// changed NewKMSKeyGeneratorWithMatDesc to NewKMSContextKeyGenerator
cipherDataGenerator := s3crypto.NewKMSContextKeyGenerator(kmsClient, cmkID, matDesc)
```

### Migrate Content Cipher Builders<a name="migrate-content-cipher-builders"></a>

Applications that use [AESCBCContentCipherBuilder](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#AESCBCContentCipherBuilder) to construct an AES/CBC content cipher must migrate to AES/GCM using [AESGCMContentCipherBuilderV2](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#AESGCMContentCipherBuilderV2)\.

Applications that use [AESGCMContentCipherBuilder](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#AESGCMContentCipherBuilder) to construct the AES/GCM content cipher must migrate to [AESGCMContentCipherBuilderV2](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#AESGCMContentCipherBuilderV2)\.

Attempting to use the deprecated content cipher builders with the V2 encryption client will result in a runtime error during the client construction\.

**Important**  
Due to limitations in the Go standard library, objects must be read into memory when performing encryption and decryption operations using AES/GCM\. Caution must be taken to ensure that your application does not experience memory allocation failures\.

 **Example: Migrate AESCBCContentCipherBuilder** 

 *Pre\-migration* 

```
contentCipherBuilder := s3crypto.AESCBCContentCipherBuilder(cipherDataGenerator, s3crypto.AESCBCPadder)
```

 *Post\-migration* 

```
contentCipherBuilder := s3crypto.AESGCMContentCipherBuilderV2(cipherDataGenerator)
```

 **Example: Migrate AESGCMContentCipherBuilder** 

 *Pre\-migration* 

```
contentCipherBuilder := s3crypto.AESGCMContentCipherBuilder(cipherDataGenerator, s3crypto.AESCBCPadder)
```

 *Post\-migration* 

```
contentCipherBuilder := s3crypto.AESGCMContentCipherBuilderV2(cipherDataGenerator)
```

### Migrate Encryption Client Constructors<a name="migrate-encryption-client-constructors"></a>

The V2 encryption client constructor adds an error interface type as an additional return parameter\. An error can be returned during V2 client construction if the client is given a deprecated [ContentCipherBuilder](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#ContentCipherBuilder) or [CipherDataGenerator](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#CipherDataGenerator)\. Review the migration steps required to migrate these types\.

 **Example** 

 *Pre\-migration* 

```
encryptionClient := s3crypto.NewEncryptionClient(sess, contentCipherBuilder)
```

 *Post\-migration* 

```
encryptionClient, err := s3crypto.NewEncryptionClientV2(sess, contentCipherBuilder)
if err != nil {
    panic(err)
}
```

### Migrate Custom Encryption Client Configurations<a name="migrate-custom-encryption-client-configurations"></a>

Clients that utilize custom client configuration options will be required to update their function argument signatures to use [EncryptionClientOptions](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#EncryptionClientOptions) for setting custom options such as an alternative [SaveStrategy](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#SaveStrategy)\.

 **Pre\-migration** 

```
// example setting an alternative SaveStrategy
encryptionClient := s3crypto.NewEncryptionClient(cipherDataGenerator, contentCipherBuilder, func(o *s3crypto.EncryptionClient) {
    // Set Instruction File Save Strategy
    o.SaveStrategy = s3crypto.S3SaveStrategy{Client: s3.New(sess)}
})
```

 **Post\-migration** 

```
// example setting an alternative SaveStrategy
encryptionClient, err := s3crypto.NewEncryptionClientV2(sess, contentCipherBuilder, func(o *s3crypto.EncryptionClientOptions) {
    // Set Instruction File Save Strategy
    o.SaveStrategy = s3crypto.S3SaveStrategy{Client: s3.New(sess)}
})
if err != nil {
    panic(err)
}
```

### Migrate Decryption Client Constructors<a name="migrate-decryption-client-constructors"></a>

The V2 decryption client now requires that an application registers the content ciphers and key wrapping algorithms that it wants to decrypt\. This is registration is done using the [CryptoRegistry](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#CryptoRegistry), and a series of registration helper functions are included to enable the V2 decryption client to decrypt objects written in either the V1 or V2 encryption formats\.

 **Step 1: Instantiate a CryptoRegistry** 

```
registry := s3crypto.NewCryptoRegistry()
```

 **Step 2: Register required content decryption algorithms** 

 *To read content encrypted using V1 AESGCMContentCipherBuilder or V2 AESGCMContentCipherBuilderV2:* 

```
if err := s3crypto.RegisterAESGCMContentCipher(registry); err != nil {
   panic(err)
}
```

 *To read content encrypted using V1 AESCBCContentCipherBuilder:* 

```
padder := s3crypto.AESCBCPadder // Use the padder provided to AESCBCContentCipherBuilder

 if err := s3crypto.RegisterAESCBCContentCipher(registry, padder); err != nil {
     panic(err)
 }
```

 *To read custom content cipher implementations:* 

If your applications implements or uses a custom content cipher implementation, you may register that implementation using the CryptoRegistry’s [AddCEK](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#CryptoRegistry.AddCEK) method\. If you require custom padders for your cipher, they can be registered using [AddPadder](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#CryptoRegistry.AddPadder)\.

```
if err := registry.AddCEK("CustomCEK", NewCustomCEK); err != nil {
    panic(err)
}

if err := registry.AddPadder("CustomPadder", NewCustomPadder); err != nil {
    panic(err)
}
```

 **Step 3: Register required key wrapping algorithms** 

 *To read keys created using the V2 NewKMSContextKeyGenerator:* 

Your application can opt to limit the CMK that is used when calling the KMS `Decrypt` API\. Two registration functions allow the selection of the desired behavior\. [RegisterKMSContextWrapWithCMK](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#RegisterKMSContextWrapWithCMK) and [RegisterKMSContextWrapWithAnyCMK](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#RegisterKMSContextWrapWithAnyCMK)\. Only one of these two methods should be used, and attempting to use both functions with a single registry will result in a runtime error\.

```
// Use RegisterKMSContextWrapWithCMK to limit the KMS Decrypt to a single CMK
if err := s3crypto.RegisterKMSContextWrapWithCMK(registry, kms.New(sess), "key-id"); err != nil {
    panic(err)
}

// Use RegisterKMSContextWrapWithAnyCMK to allow the KMS Decrypt call for any CMK
if err := s3crypto.RegisterKMSContextWrapWithAnyCMK(registry, kms.New(sess)); err != nil {
    panic(err)
}
```

 *To read keys created using the V1 NewKMSKeyGenerator or NewKMSKeyGeneratorWithMatDesc:* 

Your application can opt to limit the CMK that is used when calling the KMS Decrypt API\. Two registration functions allow the selection of the desired behavior\. [RegisterKMSWrapWithCMK](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#RegisterKMSWrapWithCMK) and [RegisterKMSWrapWithAnyCMK](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#RegisterKMSWrapWithAnyCMK)\. Use only one of these methods\. Attempting to register both functions into the registry will result in a runtime error\.

```
// Use RegisterKMSWrapWithCMK to limit the KMS Decrypt Call to a single CMK
if err := s3crypto.RegisterKMSWrapWithCMK(registry, kms.New(sess), "key-id"); err != nil {
    panic(err)
}

// Use RegisterKMSWrapWithAnyCMK to allow KMS Decrypt call for any CMK
if err := s3crypto.RegisterKMSWrapWithAnyCMK(registry, kms.New(sess)); err != nil {
    panic(err)
}
```

 *To read custom key wrapping algorithm implementations:* 

If your applications implements or uses a custom key wrapping implementation, you may register that implementation using the CryptoRegistry’s [AddWrap](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#CryptoRegistry.AddWrap) method\.

```
if err := registry.AddWrap("CustomWrap", NewCustomWrap); err != nil {
    panic(err)
}
```

 **Step 4: Construct the client** 

After registering your applications required content decryption and key wrapping algorithms to the CryptoRegistry, you can now construct a V2 decryption client using [NewDecryptionClientV2](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#NewDecryptionClientV2)\.

```
decryptionClient, err := s3crypto.NewDecryptionClientV2(sess, registry)
if err != nil {
    panic(err)
}
```

### Migrating Custom Decryption Client Configurations<a name="migrating-custom-decryption-client-configurations"></a>

Clients that use custom client configuration options are required to update their functional argument signatures to use [DecryptionClientOptions](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#DecryptionClientOptions) for setting custom options, such as an alternative [LoadStrategy](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#LoadStrategy)\.

 **Example** 

 *Pre\-migration* 

```
// example setting an alternative LoadStrategy
decryptionClient := s3crypto.NewDecryptionClient(sess, func(o *s3crypto.DecryptionClient) {
    // Set Instruction File Load Strategy
    o.LoadStrategy = s3crypto.S3LoadStrategy{Client: s3.New(sess)}
})
```

 *Post\-migration* 

```
// example setting an alternative LoadStrategy
decryptionClient, err := s3crypto.NewDecryptionClientV2(sess, registry, func(o *s3crypto.DecryptionClientOptions) {
    // Set Instruction File Load Strategy
    o.LoadStrategy = s3crypto.S3LoadStrategy{Client: s3.New(sess)}
})
if err != nil {
    panic(err)
}
```

After you complete this migration, you can proceed to testing and deployment using your application’s best practices\. After deploying your application deployment, you will have successfully migrated it from the V1 to V2 Amazon S3 encryption clients\.