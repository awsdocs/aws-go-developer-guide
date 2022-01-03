# Encrypting Amazon S3 Bucket Items<a name="s3-examples-encryption"></a>

Amazon S3 supports encrypting Amazon S3 bucket objects on both the client and the server\. To encrypt objects on the client, you perform the encryption yourself, either using keys that you create or keys that AWS Key Management Service \(AWS KMS\) manages for you\.

To encrypt objects on the server, you have more options\.
+ You can have Amazon S3 automatically encrypt objects as you upload them to a bucket\. Once you configure a bucket with this option, every object that you upload–from that point on–is encrypted\.
+ You can encrypt objects yourself before you upload them to a bucket\. The disadvantage with this approach is that you can still upload objects that are not encrypted\. We don’t show this approach in the documentation\.
+ You can have Amazon S3 reject objects that you attempt to upload to a bucket without requesting Amazon S3 encrypt the items\.

The following examples describe some of these options\.

Learn about encryption in Amazon S3 at [Protecting Data Using Encryption](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html)\.

**Topics**
+ [Setting Default Server\-Side Encryption for an Amazon S3 Bucket](s3-example-default-server-side-encryption.md)
+ [Requiring Encryption on the Server to Upload Amazon S3 Bucket Objects](s3-example-enforce-server-side-encryption.md)
+ [Encrypting an Amazon S3 Bucket Object on the Server Using AWS KMS](s3-example-server-side-encryption-with-kms.md)