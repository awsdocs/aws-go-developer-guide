# Describing CloudWatch Alarms<a name="cw-example-describing-alarms"></a>

This example shows you how to retrieve basic information that describes your CloudWatch alarms\.

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/cloudwatch) repository on GitHub\.

## Scenario<a name="cw-describe-alarms-scenario"></a>

An alarm watches a single metric over a time period you specify\. The alarm performs one or more actions based on the value of the metric relative to a given threshold over a number of time periods\.

In this example, Go code is used to describe alarms in CloudWatch\. The code uses the AWS SDK for Go to describe alarms by using this method of the `AWS.CloudWatch` client class:
+  [DescribeAlarms](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudwatch/#CloudWatch.DescribeAlarms) 

## Prerequisites<a name="cw-describe-alarms-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with CloudWatch alarms\. To learn more, see [Creating Amazon CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the Amazon CloudWatch User Guide\.

## Describe Alarms<a name="cw-example-alarms"></a>

Choose **Copy** to save the code locally\.

Create the file `describe_alarms.go`\. Import the packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/cloudwatch"

    "fmt"
    "os"
)
```

Initialize a session that the SDK will use to load credentials from the shared credentials file \~/\.aws/credentials, load your configuration from the shared configuration file \~/\.aws/config, and create a CloudWatch client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

svc := cloudwatch.New(sess)
```

Call `DescribeAlarms`, and print the results\.

```
resp, err := svc.DescribeAlarms(nil)
for _, alarm := range resp.MetricAlarms {
    fmt.Println(*alarm.AlarmName)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudwatch/describe_alarms.go) on GitHub\.