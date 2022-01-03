# Using Elastic IP Addresses in Amazon EC2<a name="ec2-example-elastic-ip-addresses"></a>

These Go examples show you how to:
+ Describe Amazon EC2 instance IP addresses
+ Allocate addresses to Amazon EC2 instances
+ Release Amazon EC2 instance IP addresses

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/ec2) repository on GitHub\.

## Scenario<a name="ec-allocate-address-scenario"></a>

An Elastic IP address is a static IP address designed for dynamic cloud computing that is associated with your AWS account\. It is a public IP address, reachable from the Internet\. If your instance doesn’t have a public IP address, you can associate an Elastic IP address with the instance to enable communication with the Internet\.

In this example, you use a series of Go routines to perform several Amazon EC2 operations involving Elastic IP addresses\. The routines use the AWS SDK for Go to manage Elastic IP addresses by using these methods of the Amazon EC2 client class:
+  [DescribeAddresses](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.DescribeAddresses) 
+  [AllocateAddress](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.AllocateAddress) 
+  [AssociateAddress](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.AssociateAddress) 
+  [ReleaseAddress](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.ReleaseAddress) 

## Prerequisites<a name="ec-allocate-address-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with Elastic IP addresses in Amazon EC2\. To learn more, see [Elastic IP Addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) in the Amazon EC2 User Guide for Linux Instances or [Elastic IP Addresses](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/elastic-ip-addresses-eip.html) in the Amazon EC2 User Guide for Windows Instances\.

## Describe Instance IP Addresses<a name="ec2-example-describe"></a>

Create a new Go file named `ec2_describe_addresses.go`\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
package main

import (
    "fmt"
    "os"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/ec2"
)
```

### Get the Address Descriptions<a name="get-the-address-descriptions"></a>

This routine prints out the Elastic IP Addresses for the account’s VPC\. Initialize a session that the SDK will use to load credentials from the shared credentials file, \~/\.aws/credentials, and create a new EC2 service client\.

```
func main() {
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create an EC2 service client.
    svc := ec2.New(sess)
```

Make the API request to EC2 filtering for the addresses in the account’s VPC\.

```
    result, err := svc.DescribeAddresses(&ec2.DescribeAddressesInput{
        Filters: []*ec2.Filter{
            {
                Name:   aws.String("domain"),
                Values: aws.StringSlice([]string{"vpc"}),
            },
        },
    })
    if err != nil {
        exitErrorf("Unable to elastic IP address, %v", err)
    }

    // Printout the IP addresses if there are any.
    if len(result.Addresses) == 0 {
        fmt.Printf("No elastic IPs for %s region\n", *svc.Config.Region)
    } else {
        fmt.Println("Elastic IPs")
        for _, addr := range result.Addresses {
            fmt.Println("*", fmtAddress(addr))
        }
    }
}
```

The `fmtAddress` and `exitErrorf` functions are utility functions used in the example\.

```
func fmtAddress(addr *ec2.Address) string {
    out := fmt.Sprintf("IP: %s,  allocation id: %s",
        aws.StringValue(addr.PublicIp), aws.StringValue(addr.AllocationId))
    if addr.InstanceId != nil {
        out += fmt.Sprintf(", instance-id: %s", *addr.InstanceId)
    }
    return out
}

func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/ec2_describe_addresses.go) on GitHub\.

## Allocate Addresses to Instances<a name="ec2-example-allocate"></a>

Create a new Go file named `ec2_allocate_address.go`\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
package main

import (
    "fmt"
    "os"
    "path/filepath"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/ec2"
)
```

This routine attempts to allocate a VPC Elastic IP address for the current region\. The IP address requires and will be associated with the instance ID that is passed in\.

```
func main() {
    if len(os.Args) != 2 {
        exitErrorf("instance ID required\nUsage: %s instance_id",
            filepath.Base(os.Args[0]))
    }
    instanceID := os.Args[1]
```

You will need to initialize a session that the SDK will use to load credentials from the shared credentials file, \~/\.aws/credentials, and create a new Amazon EC2 service client\.

```
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create an EC2 service client.
    svc := ec2.New(sess)
```

Call [AllocateAddress](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.AllocateAddress), passing in “vpc” as the Domain value\.

```
    allocRes, err := svc.AllocateAddress(&ec2.AllocateAddressInput{
        Domain: aws.String("vpc"),
    })
    if err != nil {
        exitErrorf("Unable to allocate IP address, %v", err)
    }
```

Call [AssociateAddress](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.AssociateAddress) to associate the new Elastic IP address with an existing Amazon EC2 instance, and print out the results\.

```
    assocRes, err := svc.AssociateAddress(&ec2.AssociateAddressInput{
        AllocationId: allocRes.AllocationId,
        InstanceId:   aws.String(instanceID),
    })
    if err != nil {
        exitErrorf("Unable to associate IP address with %s, %v",
            instanceID, err)
    }

    fmt.Printf("Successfully allocated %s with instance %s.\n\tallocation id: %s, association id: %s\n",
        *allocRes.PublicIp, instanceID, *allocRes.AllocationId, *assocRes.AssociationId)
}
```

This example also uses the `exitErrorf` utility function\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/ec2_allocate_address.go) on GitHub\.

## Release Instance IP Addresses<a name="ec2-example-release-instance-addresses"></a>

This routine releases an Elastic IP address allocation ID\. If the address is associated with an Amazon EC2 instance, the association is removed\.

Create a new Go file named `ec2_release_address.go`\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
package main

import (
    "fmt"
    "os"
    "path/filepath"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/awserr"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/ec2"
)
```

The routine requires that the user pass in the allocation ID of the Elastic IP address\.

```
func main() {
    if len(os.Args) != 2 {
        exitErrorf("allocation ID required\nUsage: %s allocation_id",
            filepath.Base(os.Args[0]))
    }
    allocationID := os.Args[1]
```

Initialize a session that the SDK will use to load credentials from the shared credentials file, \~/\.aws/credentials, and create a new EC2 service client\.

```
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create an EC2 service client.
    svc := ec2.New(sess)
```

Attempt to release the Elastic IP address by using the allocation ID\.

```
    _, err = svc.ReleaseAddress(&ec2.ReleaseAddressInput{
        AllocationId: aws.String(allocationID),
    })
    if err != nil {
        if aerr, ok := err.(awserr.Error); ok && aerr.Code() == "InvalidAllocationID.NotFound" {
            exitErrorf("Allocation ID %s does not exist", allocationID)
        }
        exitErrorf("Unable to release IP address for allocation %s, %v",
            allocationID, err)
    }

    fmt.Printf("Successfully released allocation ID %s\n", allocationID)
}
```

This example uses the `fmtAddress` and `exitErrorf` utility functions\.

```
func fmtAddress(addr *ec2.Address) string {
    out := fmt.Sprintf("IP: %s,  allocation id: %s",
        aws.StringValue(addr.PublicIp), aws.StringValue(addr.AllocationId))
    if addr.InstanceId != nil {
        out += fmt.Sprintf(", instance-id: %s", *addr.InstanceId)
    }
    return out
}

func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/ec2_describe_addresses.go) on GitHub\.