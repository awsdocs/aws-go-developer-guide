# AWS CloudTrail Examples Using the AWS SDK for Go<a name="using-cloudtrail-with-go-sdk"></a>

CloudTrail is an AWS service that helps you enable governance, compliance, and operational and risk auditing of your AWS account\. Actions taken by a user, role, or an AWS service are recorded as events in CloudTrail\. Events include actions taken in the AWS Management Console, AWS Command Line Interface, and AWS SDKs and APIs\.

The examples assume you have already set up and configured the SDK \(that is, you’ve imported all required packages and set your credentials and region\)\. For more information, see [Getting Started with the AWS SDK for Go](setting-up.md) and [Configuring the AWS SDK for Go](configuring-sdk.md)\.

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/cloudtrail) repository on GitHub\.

**Topics**
+ [Listing the CloudTrail Trails](cloudtrail-example-describe-trails.md)
+ [Creating a CloudTrail Trail](cloudtrail-example-create-trail.md)
+ [Listing CloudTrail Trail Events](cloudtrail-example-lookup-events.md)
+ [Deleting a CloudTrail Trail](cloudtrail-example-delete-trail.md)