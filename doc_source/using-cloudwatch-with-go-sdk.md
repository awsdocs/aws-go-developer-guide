# Amazon CloudWatch Examples Using the AWS SDK for Go<a name="using-cloudwatch-with-go-sdk"></a>

Amazon CloudWatch is a web service that monitors your AWS resources and the applications you run on AWS in real time\. You can use CloudWatch to collect and track metrics, which are variables you can measure for your resources and applications\. CloudWatch alarms send notifications or automatically make changes to the resources you are monitoring based on rules that you define\.

The AWS SDK for Go examples show you how to integrate CloudWatch into your Go applications\. The examples assume you have already set up and configured the SDK \(that is, you have imported all required packages and set your credentials and region\)\. For more information, see [Getting Started with the AWS SDK for Go](setting-up.md) and [Configuring the AWS SDK for Go](configuring-sdk.md)\.

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/cloudwatch) repository on GitHub\.

**Topics**
+ [Describing CloudWatch Alarms](cw-example-describing-alarms.md)
+ [Using Alarms and Alarm Actions in CloudWatch](cw-example-using-alarm-actions.md)
+ [Getting Metrics from CloudWatch](cw-example-getting-metrics.md)
+ [Sending Events to Amazon CloudWatch Events](cw-example-sending-events.md)
+ [Getting Log Events from CloudWatch](cw-example-getting-log-events.md)