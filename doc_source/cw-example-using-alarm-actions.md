# Using Alarms and Alarm Actions in CloudWatch<a name="cw-example-using-alarm-actions"></a>

These Go examples show you how to change the state of your Amazon EC2 instances automatically based on a CloudWatch alarm, as follows:
+ Creating and enabling actions on an alarm
+ Disabling actions on an alarm

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/cloudwatch) repository on GitHub\.

## Scenario<a name="cw-create-alarm-actions-scenario"></a>

You can use alarm actions to create alarms that automatically stop, terminate, reboot, or recover your Amazon EC2 instances\. You can use the stop or terminate actions when you no longer need an instance to be running\. You can use the reboot and recover actions to automatically reboot the instance\.

In this example, Go code is used to define an alarm action in CloudWatch that triggers the reboot of an Amazon EC2 instance\. The code uses the AWS SDK for Go to manage instances by using these methods of [PutMetricAlarm](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudwatch/#CloudWatch) type:
+  [PutMetricAlarm](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudwatch/#CloudWatch.PutMetricAlarm) 
+  [EnableAlarmActions](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudwatch/#CloudWatch.EnableAlarmActions) 
+  [DisableAlarmActions](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudwatch/#CloudWatch.DisableAlarmActions) 

## Prerequisites<a name="cw-create-alarm-actions-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with CloudWatch alarm actions\. To learn more, see [Create Alarms to Stop, Terminate, Reboot, or Recover an Instance](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/UsingAlarmActions.html) in the Amazon CloudWatch User Guide\.

## Create and Enable Actions on an Alarm<a name="cw-example-alarm-actions"></a>

Choose **Copy** to save the code locally\.

Create the file `create_enable_alarms.go`\.

Import packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/cloudwatch"

    "fmt"
    "os"
)
```

Get an instance name, value, and alarm name\.

```
if len(os.Args) != 4 {
    fmt.Println("You must supply an instance name, value, and alarm name")
    os.Exit(1)
}

instance := os.Args[1]
value := os.Args[2]
name := os.Args[3]
```

Initialize a session that the SDK will use to load credentials from the shared credentials file `~/.aws/credentials`, load your configuration from the shared configuration file `~/.aws/config`, and create a CloudWatch client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

// Create new CloudWatch client.
svc := cloudwatch.New(sess)
```

Create a metric alarm that reboots an instance if its CPU utilization is greater than 70 percent\.

```
_, err := svc.PutMetricAlarm(&cloudwatch.PutMetricAlarmInput{
    AlarmName:          aws.String(name),
    ComparisonOperator: aws.String(cloudwatch.ComparisonOperatorGreaterThanThreshold),
    EvaluationPeriods:  aws.Int64(1),
    MetricName:         aws.String("CPUUtilization"),
    Namespace:          aws.String("AWS/EC2"),
    Period:             aws.Int64(60),
    Statistic:          aws.String(cloudwatch.StatisticAverage),
    Threshold:          aws.Float64(70.0),
    ActionsEnabled:     aws.Bool(true),
    AlarmDescription:   aws.String("Alarm when server CPU exceeds 70%"),
    Unit:               aws.String(cloudwatch.StandardUnitSeconds),

    // This is apart of the default workflow actions. This one will reboot the instance, if the
    // alarm is triggered.
    AlarmActions: []*string{
        aws.String(fmt.Sprintf("arn:aws:swf:us-east-1:%s:action/actions/AWS_EC2.InstanceId.Reboot/1.0", instance)),
    },
    Dimensions: []*cloudwatch.Dimension{
        {
            Name:  aws.String("InstanceId"),
            Value: aws.String(value),
        },
    },
})
```

Call `EnableAlarmActions` with the new alarm for the instance, and display a message\.

```
result, err := svc.EnableAlarmActions(&cloudwatch.EnableAlarmActionsInput{
    AlarmNames: []*string{
        aws.String(name),
    },
})
fmt.Println("Alarm action enabled", result)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/cloudwatch/create_enable_alarms.go) on GitHub\.

## Disable Actions on an Alarm<a name="disable-actions-on-an-alarm"></a>

Choose **Copy** to save the code locally\.

Create the file `disable_alarm.go`\.

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

Get the name of the alarm from the command line\.

```
if len(os.Args) != 2 {
    fmt.Println("You must supply an alarm name")
    os.Exit(1)
}
```

Initialize a session that the SDK will use to load credentials from the shared credentials file `~/.aws/credentials`, load your configuration from the shared configuration file `~/.aws/config`, and create a CloudWatch client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

// Create new CloudWatch client.
svc := cloudwatch.New(sess)
```

Call `DisableAlarmActions` to disable the actions for this alarm and display a message\.

```
// Disable the alarm.
_, err := svc.DisableAlarmActions(&cloudwatch.DisableAlarmActionsInput{
    AlarmNames: []*string{
        aws.String(name),
    },
})
fmt.Println("Success")
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/cloudwatch/disable_alarm.go) on GitHub\.