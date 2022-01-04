# Sending Events to Amazon CloudWatch Events<a name="cw-example-sending-events"></a>

These Go examples show you how to use the AWS SDK for Go to:
+ Create and update a rule used to trigger an event
+ Define one or more targets to respond to an event
+ Send events that are matched to targets for handling

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/cloudwatch) repository on GitHub\.

## Scenario<a name="cw-get-sending-events-scenario"></a>

CloudWatch Events delivers a near real\-time stream of system events that describe changes in AWS resources to any of various targets\. Using simple rules, you can match events and route them to one or more target functions or streams\.

In these examples, Go code is used to send events to CloudWatch Events\. The code uses the AWS SDK for Go to manage instances by using these methods of the [CloudWatchEvents](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudwatchevents/#CloudWatchEvents) type:
+  [PutRule](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudwatchevents/#CloudWatchEvents.PutRule) 
+  [PutTargets](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudwatchevents/#CloudWatchEvents.PutTargets) 
+  [PutEvents](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudwatchevents/#CloudWatchEvents.PutEvents) 

## Prerequisites<a name="cw-sending-events-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with CloudWatch Events\. To learn more, see [Adding Events with PutEvents](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/AddEventsPutEvents.html) in the Amazon CloudWatch Events User Guide\.

## Tasks Before You Start<a name="tasks-before-you-start"></a>

To set up and run this example, you must first complete these tasks:

1. Create a Lambda function using the hello\-world blueprint to serve as the target for events\. To learn how, see [Step 1: Create an AWS Lambda function](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/LogEC2InstanceState.html) in the CloudWatch Events User Guide\.

1. Create an IAM role whose policy grants permission to CloudWatch Events and that includes `events.amazonaws.com` as a trusted entity\. For more information about creating an IAM role, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the IAM User Guide\.

   Use the following role policy when creating the IAM role\.

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Sid": "CloudWatchEventsFullAccess",
         "Effect": "Allow",
         "Action": "events:*",
         "Resource": "*"
      },
      {
         "Sid": "IAMPassRoleForCloudWatchEvents",
         "Effect": "Allow",
         "Action": "iam:PassRole",
         "Resource": "arn:aws:iam::*:role/AWS_Events_Invoke_Targets"
      }
   ]
}
```

Use the following trust relationship when creating the IAM role\.

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Principal": {
            "Service": "events.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
      }
   ]
}
```

## Create a Scheduled Rule<a name="cw-example-scheduled-rule"></a>

Choose **Copy** to save the code locally\.

Create the file `events_schedule_rule.go`\. Import the packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/cloudwatchevents"

    "fmt"
)
```

Initialize a session that the SDK will use to load credentials from the shared credentials file \~/\.aws/credentials, load your configuration from the shared configuration file \~/\.aws/config, and create a CloudWatch Events client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

// Create the cloudwatch events client
svc := cloudwatchevents.New(sess)
```

Call `PutRule`, supplying a name, `DEMO_EVENT`, ARN of the IAM role you created, `IAM_ROLE_ARN`, and an expression defining the schedule\. Finally, display the ARN of the rule\.

```
result, err := svc.PutRule(&cloudwatchevents.PutRuleInput{
    Name:               aws.String("DEMO_EVENT"),
    RoleArn:            aws.String("IAM_ROLE_ARN"),
    ScheduleExpression: aws.String("rate(5 minutes)"),
})
fmt.Println("Rule ARN:", result.RuleArn)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/cloudwatch/events_schedule_rule.go) on GitHub\.

## Add a Lambda Function Target<a name="add-a-lambda-function-target"></a>

Choose **Copy** to save the code locally\.

Create the file `events_put_targets.go`\. Import the packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/cloudwatchevents"

    "fmt"
)
```

Initialize a session that the SDK will use to load credentials from the shared credentials file \~/\.aws/credentials, load your configuration from the shared configuration file \~/\.aws/config, and create a new CloudWatch Events client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

// Create the cloudwatch events client
svc := cloudwatchevents.New(sess)
```

Call `PutTargets`, supplying a name for the rule, `DEMO_EVENT`\. For the target, specify the ARN of the Lambda function you created, `LAMBDA_FUNCTION_ARN`, and the ID of the rule, `myCloudWatchEventsTarget`\. Print any errors, or a success message with any targets that failed\.

```
result, err := svc.PutTargets(&cloudwatchevents.PutTargetsInput{
    Rule: aws.String("DEMO_EVENT"),
    Targets: []*cloudwatchevents.Target{
        &cloudwatchevents.Target{
            Arn: aws.String("LAMBDA_FUNCTION_ARN"),
            Id:  aws.String("myCloudWatchEventsTarget"),
        },
    },
})
fmt.Println("Success", result)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/cloudwatch/events_put_targets.go) on GitHub\.

## Send Events<a name="send-events"></a>

Choose **Copy** to save the code locally\.

Create the file `events_put_events.go`\. Import the packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/cloudwatchevents"

    "fmt"
)
```

Initialize a session that the SDK will use to load credentials from the shared credentials file \~/\.aws/credentials, load your configuration from the shared configuration file \~/\.aws/config, and create a CloudWatch Events client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

// Create the cloudwatch events client
svc := cloudwatchevents.New(sess)
```

Call `PutEvents`, supplying key\-name value pairs in the `Details` field, and specifying the ARN of the Lambda function you created, `RESOURCE_ARN`\. See [PutEventsRequestEntry](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudwatchevents/#PutEventsRequestEntry) for a description of the fields\. Finally, display the ingested events\.

```
result, err := svc.PutEvents(&cloudwatchevents.PutEventsInput{
    Entries: []*cloudwatchevents.PutEventsRequestEntry{
        &cloudwatchevents.PutEventsRequestEntry{
            Detail:     aws.String("{ \"key1\": \"value1\", \"key2\": \"value2\" }"),
            DetailType: aws.String("appRequestSubmitted"),
            Resources: []*string{
                aws.String("RESOURCE_ARN"),
            },
            Source: aws.String("com.company.myapp"),
        },
    },
})
fmt.Println("Ingested events:", result.Entries)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/cloudwatch/events_put_events.go) on GitHub\.