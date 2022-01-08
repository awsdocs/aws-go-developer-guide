# Using Regions and Availability Zones with Amazon EC2<a name="ec2-example-regions-availability-zones"></a>

These Go examples show you how to retrieve details about AWS Regions and Availability Zones\.

The code in this example uses the AWS SDK for Go to perform these tasks by using these methods of the Amazon EC2 client class:
+  [DescribeRegions](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.DescribeRegions) 
+  [DescribeAvailabilityZones](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.DescribeAvailabilityZones) 

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/ec2) repository on GitHub\.

## Scenario<a name="ec2-scenario-regions-and-azs"></a>

Amazon EC2 is hosted in multiple locations worldwide\. These locations are composed of AWS Regions and Availability Zones\. Each region is a separate geographic area with multiple, isolated locations known as Availability Zones\. Amazon EC2 provides the ability to place instances and data in these multiple locations\.

In this example, you use Go code to retrieve details about regions and Availability Zones\. The code uses the AWS SDK for Go tomanage instances by using the following methods of the Amazon EC2 client class:
+  [DescribeAvailabilityZones](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.DescribeAvailabilityZones) 
+  [DescribeRegions](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.DescribeRegions) 

## Prerequisites<a name="ec2-regions-and-azs-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with AWS Regions and Availability Zones\. To learn more, see [Regions and Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) in the Amazon EC2 User Guide for Linux Instances or [Regions and Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/resources.html) in the Amazon EC2 User Guide for Windows Instances\.

## List the Regions<a name="list-the-regions"></a>

This example lists the regions in which Amazon EC2 is available\.

To get started, create a new Go file named `regions_and_availability.go`\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
package main

import (
    "fmt"

    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/ec2"
)
```

In the `main` function, create a session with credentials from the shared credentials file, `~/.aws/credentials`, and create a new EC2 client\.

```
func main() {
    // Load session from shared config
    sess := session.Must(session.NewSessionWithOptions(session.Options{
        SharedConfigState: session.SharedConfigEnable,
    }))

    // Create new EC2 client
    svc := ec2.New(sess)
```

Print out the list of regions that work with Amazon EC2 that are returned by calling `DescribeRegions`\.

```
    resultRegions, err := svc.DescribeRegions(nil)
    if err != nil {
        fmt.Println("Error", err)
        return
    }
```

Add a call that retrieves Availability Zones only for the region of the EC2 service object\.

```
    resultAvalZones, err := svc.DescribeAvailabilityZones(nil)
    if err != nil {
        fmt.Println("Error", err)
        return
    }

    fmt.Println("Success", resultAvalZones.AvailabilityZones)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/ec2/regions_and_availability.go) on GitHub\.