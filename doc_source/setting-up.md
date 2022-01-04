# Getting Started with the AWS SDK for Go<a name="setting-up"></a>

The AWS SDK for Go requires Go 1\.5 or later\. You can view your current version of Go by running the `go version` command\. For information about installing or upgrading your version of Go, see [https://golang\.org/doc/install](https://golang.org/doc/install)\.

## Get an Amazon Account<a name="get-amazon-account"></a>

Before you can use the AWS SDK for Go, you must have an Amazon account\. See [How do I create and activate a new Amazon Web Services account?](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account) for details\.

## Install the AWS SDK for Go<a name="install-go-sdk"></a>

To install the SDK and its dependencies, run the following Go command\.

```
go get -u github.com/aws/aws-sdk-go/...
```

If you set the [Go vendor experiment](https://github.com/aws/aws-sdk-go/blob/main/README.md#installing) environment variable to `1`, you can use the following command to get the SDK\. The SDK's runtime dependencies are vendored in the `vendor/` folder\.

```
go get -u github.com/aws/aws-sdk-go
```

## Get your AWS access keys<a name="get-aws-credentials"></a>

Access keys consist of an *access key ID* and *secret access key*, which are used to sign programmatic requests that you make to AWS\. If you don't have access keys, you can create them by using the [AWS Management Console](https://console.aws.amazon.com/console/home)\. We recommend that you use IAM access keys instead of AWS root account access keys\. IAM lets you securely control access to AWS services and resources in your AWS account\.

**Note**  
To create access keys, you must have permissions to perform the required IAM actions\. For more information, see [Granting IAM User Permission to Manage Password Policy and Credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_delegate-permissions.html) in the IAM User Guide\.

### To get your access key ID and secret access key<a name="w7aab7b9b7"></a>

1. Open the [IAM console](https://console.aws.amazon.com/iam/home)\.

1. On the navigation menu, choose **Users**\.

1. Choose your IAM user name \(not the check box\)\.

1. Open the **Security credentials** tab, and then choose **Create access key**\.

1. To see the new access key, choose **Show**\. Your credentials resemble the following:
   + Access key ID: `AKIAIOSFODNN7EXAMPLE` 
   + Secret access key: `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY` 

1. To download the key pair, choose **Download \.csv file**\. Store the keys

in a secure location\.

**Important**  
Keep the keys confidential to protect your AWS account, and never email them\. Do not share them outside your organization, even if an inquiry appears to come from AWS or Amazon\.com\. *No one who legitimately represents Amazon will ever ask you for your secret key\.* 

 **Related topics** 
+  [What Is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in IAM User Guide\.
+  [AWS Security Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in Amazon Web Services General Reference\.

## Import Packages<a name="packages"></a>

After you have installed the SDK, you import AWS packages into your Go applications to use the SDK, as shown in the following example, which imports the AWS, Session, and Amazon S3 libraries:

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
)
```