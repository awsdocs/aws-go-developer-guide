# Working with Security Groups in Amazon EC2<a name="ec2-example-security-groups"></a>

These Go examples show you how to:
+ Retrieve information about your security groups
+ Create a security group to access an Amazon EC2 instance
+ Delete an existing security group

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/ec2) repository on GitHub\.

## Scenario<a name="scenario-security-groups"></a>

An Amazon EC2 security group acts as a virtual firewall that controls the traffic for one or more instances\. You add rules to each security group to allow traffic to or from its associated instances\. You can modify the rules for a security group at any time; the new rules are automatically applied to all instances that are associated with the security group\.

The code in this example uses the AWS SDK for Go to perform these tasks by using these methods of the Amazon EC2 client class:
+  [DescribeSecurityGroups](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.DescribeSecurityGroups) 
+  [AuthorizeSecurityGroupIngress](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.AuthorizeSecurityGroupIngress) 
+  [CreateSecurityGroup](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.CreateSecurityGroup) 
+  [DescribeVpcs](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.DescribeVpcs) 
+  [DeleteSecurityGroup](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.DeleteSecurityGroup) 

## Prerequisites<a name="scenario-security-groups-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with Amazon EC2 security groups\. To learn more, see [Amazon EC2 Amazon Security Groups for Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) in the Amazon EC2 User Guide for Linux Instances or [Amazon EC2 Amazon Security Groups for Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/using-network-security.html) in the Amazon EC2 User Guide for Windows Instances\.

## Describing Your Security Groups<a name="ec2-describe-security-groups"></a>

This example describes the security groups by IDs that are passed into the routine\. It takes a space separated list of group IDs as input\.

To get started, create a new Go file named `ec2_describe_security_groups.go`\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
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

In the `main` function, get the security group ID that is passed in\.

```
if len(os.Args) < 2 {
    exitErrorf("Security Group ID required\nUsage: %s group_id ...",
        filepath.Base(os.Args[0]))
}
groupIds := os.Args[1:]
```

Initialize a session and create an EC2 service client\.

```
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Create an EC2 service client.
svc := ec2.New(sess)
```

Obtain and print out the security group descriptions\. You will explicitly check for errors caused by an invalid group ID\.

```
result, err := svc.DescribeSecurityGroups(&ec2.DescribeSecurityGroupsInput{
    GroupIds: aws.StringSlice(groupIds),
})
if err != nil {
    if aerr, ok := err.(awserr.Error); ok {
        switch aerr.Code() {
        case "InvalidGroupId.Malformed":
            fallthrough
        case "InvalidGroup.NotFound":
            exitErrorf("%s.", aerr.Message())
        }
    }
    exitErrorf("Unable to get descriptions for security groups, %v", err)
}

fmt.Println("Security Group:")
for _, group := range result.SecurityGroups {
    fmt.Println(group)
}
```

The following utility function is used by this example to display errors\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/ec2/ec2_describe_security_groups.go) on GitHub\.

## Creating a Security Group<a name="create-security-group"></a>

You can create new Amazon EC2 security groups\. To do this, you use the [CreateSecurityGroup](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.CreateSecurityGroup) method\.

This example creates a new security group with the given name and description for access to open ports 80 and 22\. If a VPC ID is not provided, it associates the security group with the first VPC in the account\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "flag"
    "fmt"
    "os"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/awserr"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/ec2"
)
```

Get the parameters \(name, description, and optional ID of the VPC\) that are passed in to the routine\.

```
namePtr := flag.String("n", "", "Group Name")
descPtr := flag.String("d", "", "Group Description")
vpcIDPtr := flag.String("vpc", "", "(Optional) VPC ID to associate security group with")

flag.Parse()

if *namePtr == "" || *descPtr == "" {
    flag.PrintDefaults()
    exitErrorf("Group name and description require")
}
```

Create a session and Amazon EC2 client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

svc := ec2.New(sess)
```

Create an Amazon EC2 client\. If the VPC ID was not provided, retrieve the first one in the account\.

```
if *vpcIDPtr == "" {
    // Get a list of VPCs so we can associate the group with the first VPC.
    result, err := svc.DescribeVpcs(nil)
    if err != nil {
        exitErrorf("Unable to describe VPCs, %v", err)
    }
    if len(result.Vpcs) == 0 {
        exitErrorf("No VPCs found to associate security group with.")
    }

    *vpcIDPtr = aws.StringValue(result.Vpcs[0].VpcId)
}
```

Create the security group with the VPC ID, name, and description\.

```
createRes, err := svc.CreateSecurityGroup(&ec2.CreateSecurityGroupInput{
    GroupName:   aws.String(*namePtr),
    Description: aws.String(*descPtr),
    VpcId:       aws.String(*vpcIDPtr),
})
if err != nil {
    if aerr, ok := err.(awserr.Error); ok {
        switch aerr.Code() {
        case "InvalidVpcID.NotFound":
            exitErrorf("Unable to find VPC with ID %q.", *vpcIDPtr)
        case "InvalidGroup.Duplicate":
            exitErrorf("Security group %q already exists.", *namePtr)
        }
    }
    exitErrorf("Unable to create security group %q, %v", *namePtr, err)
}

fmt.Printf("Created security group %s with VPC %s.\n",
    aws.StringValue(createRes.GroupId), *vpcIDPtr)
```

Add permissions to the security group\.

```
_, err = svc.AuthorizeSecurityGroupIngress(&ec2.AuthorizeSecurityGroupIngressInput{
    GroupName: aws.String(*namePtr),
    IpPermissions: []*ec2.IpPermission{
        // Can use setters to simplify seting multiple values without the
        // needing to use aws.String or associated helper utilities.
        (&ec2.IpPermission{}).
            SetIpProtocol("tcp").
            SetFromPort(80).
            SetToPort(80).
            SetIpRanges([]*ec2.IpRange{
                {CidrIp: aws.String("0.0.0.0/0")},
            }),
        (&ec2.IpPermission{}).
            SetIpProtocol("tcp").
            SetFromPort(22).
            SetToPort(22).
            SetIpRanges([]*ec2.IpRange{
                (&ec2.IpRange{}).
                    SetCidrIp("0.0.0.0/0"),
            }),
    },
})
if err != nil {
    exitErrorf("Unable to set security group %q ingress, %v", *namePtr, err)
}

fmt.Println("Successfully set security group ingress")
```

The following utility function is used by this example\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/ec2/ec2_create_security_group.go) on GitHub\.

## Deleting a Security Group<a name="delete-security-group"></a>

You can delete an Amazon EC2 security group in code\. To do this, you use the [DeleteSecurityGroup](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.DeleteSecurityGroup) method\.

This example deletes a security group with the given group ID\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
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

Get the group ID that is passed in to the routine\.

```
if len(os.Args) != 2 {
    exitErrorf("Security Group ID required\nUsage: %s group_id",
        filepath.Base(os.Args[0]))
}
groupID := os.Args[1]
```

Create a session\.

```
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Create an EC2 service client.
svc := ec2.New(sess)
```

Then delete the security group with the group ID that is passed in\.

```
_, err = svc.DeleteSecurityGroup(&ec2.DeleteSecurityGroupInput{
    GroupId: aws.String(groupID),
})
if err != nil {
    if aerr, ok := err.(awserr.Error); ok {
        switch aerr.Code() {
        case "InvalidGroupId.Malformed":
            fallthrough
        case "InvalidGroup.NotFound":
            exitErrorf("%s.", aerr.Message())
        }
    }
    exitErrorf("Unable to get descriptions for security groups, %v.", err)
}

fmt.Printf("Successfully delete security group %q.\n", groupID)
```

This example uses the following utility function\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/ec2/ec2_delete_security_group.go) on GitHub\.