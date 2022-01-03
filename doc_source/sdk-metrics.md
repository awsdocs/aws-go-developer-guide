# SDK Metrics in the AWS SDK for Go<a name="sdk-metrics"></a>

AWS SDK Metrics for Enterprise Support \(SDK Metrics\) enables enterprise customers to collect metrics from AWS SDKs on their hosts and clients shared with AWS Enterprise Support\. SDK Metrics provides information that helps speed up detection and diagnosis of issues occurring in connections to AWS services for AWS Enterprise Support customers\.

As telemetry is collected on each host, it is relayed via UDP to localhost, where the CloudWatch agent aggregates the data and sends it to the SDK Metrics service\. Therefore, to receive metrics, you must add the CloudWatch agent to your instance\.

The following topics describe how to authorize, set up and configure, and define SDK Metrics in the AWS SDK for Go\.

**Topics**
+ [Authorize SDK Metrics](authorize-metrics.md)
+ [Set Up SDK Metrics](setup-metrics.md)
+ [SDK Metric Definitions](define-metrics.md)