# Creating Amazon EC2 Instances with Tags or without Block Devices<a name="ec2-example-create-images"></a>

This Go example shows you how to:
+ Create an Amazon EC2 instance with tags or set up an instance without a block device

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/ec2) repository on GitHub\.

## Scenarios<a name="ec2-tags-scenario"></a>

In these examples, you use a series of Go routines to create Amazon EC2 instances with tags or set up an instance without a block device\.

The routines use the AWS SDK for Go to perform these tasks by using these methods of the [EC2](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2) type:
+  [BlockDeviceMapping](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#BlockDeviceMapping) 
+  [RunInstances](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.RunInstances) 
+  [CreateTags](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.CreateTags) 

## Prerequisites<a name="ec2-tags-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.

## Create an Instance with Tags<a name="create-an-instance-with-tags"></a>

The Amazon EC2 service has an operation for creating instances \([RunInstances](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.RunInstances)\) and another for attaching tags to instances \([CreateTags](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.CreateTags)\)\. To create an instance with tags, call both of these operations in succession\. The following example creates an instance and then adds a `Name` tag to it\. The Amazon EC2 console displays the value of the `Name` tag in its list of instances\.

```
package main

import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/ec2"

    "fmt"
    "log"
)

func main() {
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create EC2 service client
    svc := ec2.New(sess)

    // Specify the details of the instance that you want to create.
    runResult, err := svc.RunInstances(&ec2.RunInstancesInput{
        // An Amazon Linux AMI ID for t2.micro instances in the us-west-2 region
        ImageId:      aws.String("ami-e7527ed7"),
        InstanceType: aws.String("t2.micro"),
        MinCount:     aws.Int64(1),
        MaxCount:     aws.Int64(1),
    })

    if err != nil {
        fmt.Println("Could not create instance", err)
        return
    }

    fmt.Println("Created instance", *runResult.Instances[0].InstanceId)

    // Add tags to the created instance
    _, errtag := svc.CreateTags(&ec2.CreateTagsInput{
        Resources: []*string{runResult.Instances[0].InstanceId},
        Tags: []*ec2.Tag{
            {
                Key:   aws.String("Name"),
                Value: aws.String("MyFirstInstance"),
            },
        },
    })
    if errtag != nil {
        log.Println("Could not create tags for instance", runResult.Instances[0].InstanceId, errtag)
        return
    }

    fmt.Println("Successfully tagged instance")
}
```

You can add up to 10 tags to an instance in a single `CreateTags` operation\.

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/create_instance.go) on GitHub\.

## Create an Image without a Block Device<a name="create-image-without-block-device"></a>

Sometimes when you create an Amazon EC2 image, you might want to explicitly exclude certain block devices\. To do this, you can use the `NoDevice` parameter in [BlockDeviceMapping](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#BlockDeviceMapping)\. When this parameter is set to an empty string `""`, the named device isn’t mapped\.

The `NoDevice` parameter is compatible only with `DeviceName`, not with any other field in `BlockDeviceMapping`\. The request will fail if other parameters are present\.

```
package main

import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/ec2"

    "fmt"
)

func main() {
    // Load session from shared config
    sess := session.Must(session.NewSessionWithOptions(session.Options{
        SharedConfigState: session.SharedConfigEnable,
    }))

    // Create EC2 service client
    svc := ec2.New(sess)

    opts := &ec2.CreateImageInput{
        Description: aws.String("image description"),
        InstanceId:  aws.String("i-abcdef12"),
        Name:        aws.String("image name"),
        BlockDeviceMappings: []*ec2.BlockDeviceMapping{
            {
                DeviceName: aws.String("/dev/sda1"),
                NoDevice:    aws.String(""),
            },
            {
                DeviceName: aws.String("/dev/sdb"),
                NoDevice:    aws.String(""),
            },
            {
                DeviceName: aws.String("/dev/sdc"),
                NoDevice:    aws.String(""),
            },
        },
    }
    resp, err := svc.CreateImage(opts)
    if err != nil {
        fmt.Println(err)
        return
    }

    fmt.Println("ID: ", resp.ImageId)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/create_image_no_block_device.go) on GitHub\.