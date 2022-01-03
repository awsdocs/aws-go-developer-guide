# Getting Metrics from CloudWatch<a name="cw-example-getting-metrics"></a>

These Go examples show you how to retrieve a list of published CloudWatch metrics and publish data points to CloudWatch metrics with the AWS SDK for Go, as follows:
+ Listing metrics
+ Submitting custom metrics

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/cloudwatch) repository on GitHub\.

## Scenario<a name="cw-get-metrics-scenario"></a>

Metrics are data about the performance of your systems\. You can enable detailed monitoring of some resources, such as your Amazon EC2 instances, or your own application metrics\.

In this example, Go code is used to get metrics from CloudWatch and to send events to CloudWatch Events\. The code uses the AWS SDK for Go to get metrics from CloudWatch by using these methods of the CloudWatch type:
+  [ListMetrics](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudwatch/#CloudWatch.ListMetrics) 
+  [PutMetricData](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudwatch/#CloudWatch.PutMetricData) 

## Prerequisites<a name="cw-get-metrics-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with CloudWatch metrics\. To learn more, see [Using Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html) in the Amazon CloudWatch User Guide\.

## List Metrics<a name="cw-example-list-metrics"></a>

Choose **Copy** to save the code locally\.

Create the file `listing_metrics.go`\.

Import the packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/cloudwatch"

    "fmt"
    "os"
)
```

Get the metric name, namespace, and dimension name from the command line\.

```
if len(os.Args) != 4 {
    fmt.Println("You must supply a metric name, namespace, and dimension name")
    os.Exit(1)
}

metric := os.Args[1]
namespace := os.Args[2]
dimension := os.Args[3]
```

Initialize a session that the SDK will use to load credentials from the shared credentials file \~/\.aws/credentials, load your configuration from the shared configuration file \~/\.aws/config, and create a CloudWatch client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

// Create CloudWatch client
svc := cloudwatch.New(sess)
```

Call `ListMetrics`, supplying the metric name, namespace, and dimension name\. Print the metrics returned in the result\.

```
result, err := svc.ListMetrics(&cloudwatch.ListMetricsInput{
    MetricName: aws.String(metric),
    Namespace:  aws.String(namespace),
    Dimensions: []*cloudwatch.DimensionFilter{
        &cloudwatch.DimensionFilter{
            Name: aws.String(dimension),
        },
    },
})
fmt.Println("Metrics", result.Metrics)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudwatch/listing_metrics.go) on GitHub\.

## Submit Custom Metrics<a name="cw-example-custom-metrics"></a>

Choose **Copy** to save the code locally\.

Create the file `custom_metrics.go`\.

Import the packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/cloudwatch"

    "fmt"
)
```

Initialize a session that the SDK will use to load credentials from the shared credentials file \~/\.aws/credentials, load your configuration from the shared configuration file \~/\.aws/config, and create a CloudWatch client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

// Create new cloudwatch client.
svc := cloudwatch.New(sess)
```

Call `PutMetricData` with the the custom namespace “Site/Traffic”\. The namespace has two custom dimensions: “SiteName” and “PageURL”\. “SiteName” has the value “example\.com”, the “UniqueVisitors” value 5885 and the “UniqueVisits” value 8628\. “PageURL” has the value “my\-page\.html”, and a “PageViews” value 18057\.

```
_, err := svc.PutMetricData(&cloudwatch.PutMetricDataInput{
    Namespace: aws.String("Site/Traffic"),
    MetricData: []*cloudwatch.MetricDatum{
        &cloudwatch.MetricDatum{
            MetricName: aws.String("UniqueVisitors"),
            Unit:       aws.String("Count"),
            Value:      aws.Float64(5885.0),
            Dimensions: []*cloudwatch.Dimension{
                &cloudwatch.Dimension{
                    Name:  aws.String("SiteName"),
                    Value: aws.String("example.com"),
                },
            },
        },
        &cloudwatch.MetricDatum{
            MetricName: aws.String("UniqueVisits"),
            Unit:       aws.String("Count"),
            Value:      aws.Float64(8628.0),
            Dimensions: []*cloudwatch.Dimension{
                &cloudwatch.Dimension{
                    Name:  aws.String("SiteName"),
                    Value: aws.String("example.com"),
                },
            },
        },
        &cloudwatch.MetricDatum{
            MetricName: aws.String("PageViews"),
            Unit:       aws.String("Count"),
            Value:      aws.Float64(18057.0),
            Dimensions: []*cloudwatch.Dimension{
                &cloudwatch.Dimension{
                    Name:  aws.String("PageURL"),
                    Value: aws.String("my-page.html"),
                },
            },
        },
    },
})
```

If there are any errors, print them out, otherwise list some information about the custom metrics\.

```
if err != nil {
    fmt.Println("Error adding metrics:", err.Error())
    return
}

// Get information about metrics
result, err := svc.ListMetrics(&cloudwatch.ListMetricsInput{
    Namespace: aws.String("Site/Traffic"),
})
if err != nil {
    fmt.Println("Error getting metrics:", err.Error())
    return
}

for _, metric := range result.Metrics {
    fmt.Println(*metric.MetricName)

    for _, dim := range metric.Dimensions {
        fmt.Println(*dim.Name + ":", *dim.Value)
        fmt.Println()
    }
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudwatch/custom_metrics.go) on GitHub\.