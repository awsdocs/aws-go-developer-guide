# Managing Amazon EC2 Instances<a name="ec2-example-manage-instances"></a>

These Go examples show you how to:
+ Describe Amazon EC2 instances
+ Manage Amazon EC2 instance monitoring
+ Start and stop Amazon EC2 instances
+ Reboot Amazon EC2 instances

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/ec2) repository on GitHub\.

## Scenario<a name="ec2-manage-instances-scenario"></a>

In these examples, you use a series of Go routines to perform several basic instance management operations\.

The routines use the AWS SDK for Go to perform the operations by using these methods of the Amazon EC2 client class:
+  [DescribeInstances](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.DescribeInstances) 
+  [MonitorInstances](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.MonitorInstances) 
+  [UnmonitorInstances](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.UnmonitorInstances) 
+  [StartInstances](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.StartInstances) 
+  [StopInstances](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.StopInstances) 
+  [RebootInstances](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.RebootInstances) 

## Prerequisites<a name="ec2-manage-instances-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with the lifecycle of Amazon EC2 instances\. To learn more, see [Instance Lifecycle](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html) in the Amazon EC2 User Guide for Linux Instances\.

## Describe Your Instances<a name="describing-your-instances"></a>

Create a new Go file named `describing_instances.go`\.

The Amazon EC2 service has an operation for describing instances, [DescribeInstances](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.DescribeInstances)\.

Import the required AWS SDK for Go packages\.

```
package main

import (
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/ec2"

    "fmt"
)
```

Use the following code to create a session and Amazon EC2 client\.

```
// Load session from shared config
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

// Create new EC2 client
ec2Svc := ec2.New(sess)
```

Call `DescribeInstances` to get detailed information for each instance\.

```
// Call to get detailed information on each instance
result, err := ec2Svc.DescribeInstances(nil)
if err != nil {
    fmt.Println("Error", err)
} else {
    fmt.Println("Success", result)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/ec2/describing_instances.go) on GitHub\.

## Manage Instance Monitoring<a name="ec2-manage-instance-monitoring"></a>

Create a new Go file named `monitoring_instances.go`\.

Import the required AWS SDK for Go packages\.

```
package main

import (
    "fmt"
    "os"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/awserr"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/ec2"
)
```

To access Amazon EC2, create an EC2 client\.

```
func main() {
    // Load session from shared config
    sess := session.Must(session.NewSessionWithOptions(session.Options{
        SharedConfigState: session.SharedConfigEnable,
    }))

    // Create new EC2 client
    svc := ec2.New(sess)
```

Based on the value of a command\-line argument \(ON or OFF\), call either the `MonitorInstances` method of the Amazon EC2 service object to begin detailed monitoring of the specified instances, or the `UnmonitorInstances` method\. Before you try to change the monitoring of these instances, use the `DryRun` parameter to test whether you have permission to change instance monitoring\.

```
    if os.Args[1] == "ON" {
        input := &ec2.MonitorInstancesInput{
            InstanceIds: []*string{
                aws.String(os.Args[2]),
            },
            DryRun: aws.Bool(true),
        }
        result, err := svc.MonitorInstances(input)
        awsErr, ok := err.(awserr.Error)

        if ok && awsErr.Code() == "DryRunOperation" {
            input.DryRun = aws.Bool(false)
            result, err = svc.MonitorInstances(input)
            if err != nil {
                fmt.Println("Error", err)
            } else {
                fmt.Println("Success", result.InstanceMonitorings)
            }
        } else {
            fmt.Println("Error", err)
        }
    } else if os.Args[1] == "OFF" { // Turn monitoring off
        input := &ec2.UnmonitorInstancesInput{
            InstanceIds: []*string{
                aws.String(os.Args[2]),
            },
            DryRun: aws.Bool(true),
        }
        result, err := svc.UnmonitorInstances(input)
        awsErr, ok := err.(awserr.Error)
        if ok && awsErr.Code() == "DryRunOperation" {
            input.DryRun = aws.Bool(false)
            result, err = svc.UnmonitorInstances(input)
            if err != nil {
                fmt.Println("Error", err)
            } else {
                fmt.Println("Success", result.InstanceMonitorings)
            }
        } else {
            fmt.Println("Error", err)
        }
    }

}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/ec2/monitoring_instances.go) on GitHub\.

## Start and Stop Instances<a name="scenario-starting-stopping"></a>

Create a new Go file named `start_stop_instances.go`\.

Import the required AWS SDK for Go packages\.

```
package main

import (
    "fmt"
    "os"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/awserr"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/ec2"
)
```

To access Amazon EC2, create an EC2 client\. The user will pass in a state value of START or STOP and the instance ID\.

```
func main() {
    // Load session from shared config
    sess := session.Must(session.NewSessionWithOptions(session.Options{
        SharedConfigState: session.SharedConfigEnable,
    }))

    // Create new EC2 client
    svc := ec2.New(sess)
```

Based on the value of a command\-line argument \(START or STOP\), call either the `StartInstances` method of the Amazon EC2 service object to start the specified instances, or the `StopInstances` method to stop them\. Before you try to start or stop the selected instances, use the `DryRun` parameter to test whether you have permission to start or stop them\.

```
    if os.Args[1] == "START" {
        input := &ec2.StartInstancesInput{
            InstanceIds: []*string{
                aws.String(os.Args[2]),
            },
            DryRun: aws.Bool(true),
        }
        result, err := svc.StartInstances(input)
        awsErr, ok := err.(awserr.Error)

        if ok && awsErr.Code() == "DryRunOperation" {
            // Let's now set dry run to be false. This will allow us to start the instances
            input.DryRun = aws.Bool(false)
            result, err = svc.StartInstances(input)
            if err != nil {
                fmt.Println("Error", err)
            } else {
                fmt.Println("Success", result.StartingInstances)
            }
        } else { // This could be due to a lack of permissions
            fmt.Println("Error", err)
        }
    } else if os.Args[1] == "STOP" { // Turn instances off
        input := &ec2.StopInstancesInput{
            InstanceIds: []*string{
                aws.String(os.Args[2]),
            },
            DryRun: aws.Bool(true),
        }
        result, err := svc.StopInstances(input)
        awsErr, ok := err.(awserr.Error)
        if ok && awsErr.Code() == "DryRunOperation" {
            input.DryRun = aws.Bool(false)
            result, err = svc.StopInstances(input)
            if err != nil {
                fmt.Println("Error", err)
            } else {
                fmt.Println("Success", result.StoppingInstances)
            }
        } else {
            fmt.Println("Error", err)
        }
    }
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/ec2/start_stop_instances.go) on GitHub\.

## Reboot Instances<a name="ec2-reboot-instances"></a>

Create a new Go file named `reboot_instances.go`\.

Import the required AWS SDK for Go packages\.

```
package main

import (
    "fmt"
    "os"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/awserr"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/ec2"
)
```

To access Amazon EC2, create an EC2 client\. The user will pass in a state value of START or STOP and the instance ID\.

```
    sess := session.Must(session.NewSessionWithOptions(session.Options{
        SharedConfigState: session.SharedConfigEnable,
    }))

    // Create new EC2 client
    svc := ec2.New(sess)
```

Based on the value of a command\-line argument \(START or STOP\), call either the `StartInstances` method of the Amazon EC2 service object to start the specified instances, or the `StopInstances` method to stop them\. Before you try to reboot the selected instances, use the `DryRun` parameter to test whether the instance exists and that you have permission to reboot it\.

```
    input := &ec2.RebootInstancesInput{
        InstanceIds: []*string{
            aws.String(os.Args[1]),
        },
        DryRun: aws.Bool(true),
    }
    result, err := svc.RebootInstances(input)
    awsErr, ok := err.(awserr.Error)
```

If the error code is `DryRunOperation`, it means that you do have the permissions you need to reboot the instance\.

```
    if ok && awsErr.Code() == "DryRunOperation" {
        input.DryRun = aws.Bool(false)
        result, err = svc.RebootInstances(input)
        if err != nil {
            fmt.Println("Error", err)
        } else {
            fmt.Println("Success", result)
        }
    } else { // This could be due to a lack of permissions
        fmt.Println("Error", err)
    }
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/ec2/reboot_instances.go) on GitHub\.