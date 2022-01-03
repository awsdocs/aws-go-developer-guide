# Working with Amazon EC2 Key Pairs<a name="ec2-example-working-with-key-pairs"></a>

These Go examples show you how to:
+ Describe an Amazon EC2 key pair
+ Create an Amazon EC2 key pair
+ Delete an Amazon EC2 key pair

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/ec2) repository on GitHub\.

## Scenario<a name="ec2-key-pairs-scenario"></a>

Amazon EC2 uses public–key cryptography to encrypt and decrypt login information\. Public–key cryptography uses a public key to encrypt data, then the recipient uses the private key to decrypt the data\. The public and private keys are known as a key pair\.

The routines use the AWS SDK for Go to perform these tasks by using these methods of the [EC2](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2) type:
+  [CreateKeyPair](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.CreateKeyPair) 
+  [DeleteKeyPair](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.DeleteKeyPair) 
+  [DescribeKeyPairs](https://docs.aws.amazon.com/sdk-for-go/api/service/ec2/#EC2.DescribeKeyPairs) 

## Prerequisites<a name="ec2-key-pairs-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the SDK\.
+ You are familiar with Amazon EC2 key pairs\. To learn more, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the Amazon EC2 User Guide for Linux Instances\.

## Describe Your Key Pairs<a name="example-describing-your-key-pairs"></a>

Create a new Go file named `ec2_describe_keypairs.go`\.

Import the required AWS SDK for Go packages\.

```
package main

import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/ec2"

    "fmt"
    "os"
)
```

Use the following code to create a session and Amazon EC2 client\.

```
//    go run ec2_describe_keypairs.go
    // credentials from the shared credentials file ~/.aws/credentials.
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create an EC2 service client.
```

Call `DescribeKeyPairs` to get a list of key pairs and print them out\.

```
    //  Returns a list of key pairs
    result, err := svc.DescribeKeyPairs(nil)
    if err != nil {
        exitErrorf("Unable to get key pairs, %v", err)
    }

    fmt.Println("Key Pairs:")
    for _, pair := range result.KeyPairs {
        fmt.Printf("%s: %s\n", *pair.KeyName, *pair.KeyFingerprint)
    }
```

The routine uses the following utility function\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/ec2_describe_keypairs.go) on GitHub\.

## Create a Key Pair<a name="example-create-key-pair"></a>

Create a new Go file named `ec2_create_keypair.go`\.

Import the required AWS SDK for Go packages\.

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

Get the key pair name passed in to the code and, to access Amazon EC2, create an EC2 client\.

```
func main() {
    if len(os.Args) != 2 {
        exitErrorf("pair name required\nUsage: %s key_pair_name",
            filepath.Base(os.Args[0]))
    }
    pairName := os.Args[1]
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create an EC2 service client.
    svc := ec2.New(sess)
```

Create a new key pair with the provided name\.

```
    result, err := svc.CreateKeyPair(&ec2.CreateKeyPairInput{
        KeyName: aws.String(pairName),
    })
    if err != nil {
        if aerr, ok := err.(awserr.Error); ok && aerr.Code() == "InvalidKeyPair.Duplicate" {
            exitErrorf("Keypair %q already exists.", pairName)
        }
        exitErrorf("Unable to create key pair: %s, %v.", pairName, err)
    }

    fmt.Printf("Created key pair %q %s\n%s\n",
        *result.KeyName, *result.KeyFingerprint,
        *result.KeyMaterial)
}
```

The routine uses the following utility function\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/ec2_create_keypair.go) on GitHub\.

## Delete a Key Pair<a name="example-delete-key-pair"></a>

Create a new Go file named `ec2_delete_keypair.go`\.

Import the required AWS SDK for Go packages\.

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

Get the key pair name passed in to the code and, to access Amazon EC2, create an EC2 client\.

```
func main() {
    if len(os.Args) != 2 {
        exitErrorf("pair name required\nUsage: %s key_pair_name",
            filepath.Base(os.Args[0]))
    }
    pairName := os.Args[1]

    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create an EC2 service client.
    svc := ec2.New(sess)
```

Delete the key pair with the provided name\.

```
    _, err = svc.DeleteKeyPair(&ec2.DeleteKeyPairInput{
        KeyName: aws.String(pairName),
    })
    if err != nil {
        if aerr, ok := err.(awserr.Error); ok && aerr.Code() == "InvalidKeyPair.Duplicate" {
            exitErrorf("Key pair %q does not exist.", pairName)
        }
        exitErrorf("Unable to delete key pair: %s, %v.", pairName, err)
    }

    fmt.Printf("Successfully deleted %q key pair\n", pairName)
}
```

The routine uses the following utility function\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/ec2_delete_keypair.go) on GitHub\.