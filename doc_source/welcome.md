# What is the AWS SDK for Go<a name="welcome"></a>

The AWS SDK for Go provides APIs and utilities that developers can use to build Go applications that use AWS services, such as Amazon Elastic Compute Cloud \(Amazon EC2\) and Amazon Simple Storage Service \(Amazon S3\)\.

**Note**  
This document is for version 1 of the AWS SDK for Go\. If you're looking for version 2 of the SDK, see the [version 2 developer guide](https://aws.github.io/aws-sdk-go-v2/docs/) and the [version 2 SDK API reference](https://pkg.go.dev/github.com/aws/aws-sdk-go-v2)\.

The SDK removes the complexity of coding directly against a web service interface\. It hides a lot of the lower\-level plumbing, such as authentication, request retries, and error handling\.

The SDK also includes helpful utilities\. For example, the Amazon S3 download and upload manager can automatically break up large objects into multiple parts and transfer them in parallel\.

Use the AWS SDK for Go Developer Guide to help you install, configure, and use the SDK\. The guide provides configuration information, sample code, and an introduction to the SDK utilities\.

## Using the AWS SDK for Go with AWS Cloud9<a name="using-the-aws-sdk-for-go-with-aws-cloud9"></a>

AWS Cloud9 is a web\-based integrated development environment \(IDE\) that contains a collection of tools that you use to code, build, run, test, debug, and release software in the cloud\.

See [Using AWS Cloud9 with the AWS SDK for Go](cloud9-go.md) for information on using AWS Cloud9 with the AWS SDK for Go\.

## More Info<a name="more-info"></a>
+ To learn about everything you need before you can start using the AWS SDK for Go, see [Getting Started with the AWS SDK for Go](setting-up.md)\.
+ For code examples, see [AWS SDK for Go Code Examples](common-examples.md)\.
+ You can browse the AWS SDK for Go examples in the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples) repo on GitHub\.
+ To learn about the SDK utilities, see [Using the AWS SDK for Go Utilities](sdk-utilities.md)\.
+ For learn about the types and functionality that the library provides, see the [AWS SDK for Go API Reference](https://docs.aws.amazon.com/sdk-for-go/api/)\.
+ To view a video introduction of the SDK and a sample application demonstration, see [AWS SDK for Go: Gophers Get Going with AWS](https://www.youtube.com/watch?v=iOGIKG3EptI&amp;feature=youtu.be) from AWS re:Invent 2015\.

## Maintenance and support for SDK major versions<a name="maintenance-and-support-for-sdk-major-versions"></a>

### Maintenance and support for SDK major versions<a name="sdks-major-versions-maintenance-support"></a>

For information about maintenance and support for SDK major versions and their underlying dependencies, see the following in the [AWS SDKs and Tools Reference Guide](https://docs.aws.amazon.com/sdkref/latest/guide/overview.html):
+ [AWS SDKs and tools maintenance policy](https://docs.aws.amazon.com/sdkref/latest/guide/maint-policy.html)
+ [AWS SDKs and tools version support matrix](https://docs.aws.amazon.com/sdkref/latest/guide/version-support-matrix.html)