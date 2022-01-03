# Working with IAM Server Certificates<a name="iam-example-server-certificates"></a>

This Go example shows you how to carry out basic tasks for managing server certificate HTTPS connections with the AWS SDK for Go\.

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/iam) repository on GitHub\.

## Scenario<a name="iam-certificates-scenario"></a>

To enable HTTPS connections to your website or application on AWS, you need an SSL/TLS server certificate\. To use a certificate that you obtained from an external provider with your website or application on AWS, you must upload the certificate to IAM or import it into AWS Certificate Manager\.

In this example, you use a series of Go routines to manage policies in IAM\. The routines use the AWS SDK for GoIAM client methods that follow:
+  [ListServerCertificates](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.ListServerCertificates) 
+  [GetServerCertificate](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.GetServerCertificate) 
+  [UpdateServerCertificate](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.UpdateServerCertificate) 
+  [DeleteServerCertificate](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.DeleteServerCertificate) 

## Prerequisites<a name="iam-certificates-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with server certificates\. To learn more, see [Working with Server Certificates](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_server-certs.html) in the IAM User Guide\.

## List Your Server Certificates<a name="iam-example-list-certificates"></a>

This code lists your certificates\. Create a new Go file named `iam_listservercerts.go`\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "fmt"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/iam"
)
```

Set up the session and IAM client\.

```
func main() {
    // Initialize a session in us-west-2 that the SDK will use to load
    // credentials from the shared credentials file ~/.aws/credentials.
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create a IAM service client.
    svc := iam.New(sess)
```

Call `ListServerCertificates` and print the details\.

```
    result, err := svc.ListServerCertificates(nil)
    if err != nil {
        fmt.Println("Error", err)
        return
    }

    for i, metadata := range result.ServerCertificateMetadataList {
        if metadata == nil {
            continue
        }

        fmt.Printf("Metadata %d: %v\n", i, metadata)
    }
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_listservercerts.go) on GitHub\.

## Get a Server Certificate<a name="iam-example-get-certificate"></a>

In this example, you retrieve an existing server certificate\.

Create a new Go file named `iam_getservercert.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
package main

import (
    "fmt"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/iam"
)
```

Set up a new IAM client\.

```
func main() {
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create a IAM service client.
    svc := iam.New(sess)
```

Call `GetServerCertificate`, passing the name of the certificate, and print the results\.

```
    result, err := svc.GetServerCertificate(&iam.GetServerCertificateInput{
        ServerCertificateName: aws.String("CERTIFICATE_NAME"),
    })
    if err != nil {
        fmt.Println("Error", err)
        return
    }

    fmt.Println("ServerCertificate:", result)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_getservercert.go) on GitHub\.

## Update a Server Certificate<a name="iam-example-update-certificate"></a>

In this example, you update an existing server certificate\.

Create a new Go file named `iam_updateservercert.go`\. You call the `UpdateServerCertificate` method of the IAM service object to change the name of the certificate\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
package main

import (
    "fmt"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/iam"
)
```

Set up a new IAM client\.

```
func main() {
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create a IAM service client.
    svc := iam.New(sess)
```

Update the certificate name\.

```
    _, err = svc.UpdateServerCertificate(&iam.UpdateServerCertificateInput{
        ServerCertificateName:    aws.String("CERTIFICATE_NAME"),
        NewServerCertificateName: aws.String("NEW_CERTIFICATE_NAME"),
    })
    if err != nil {
        fmt.Println("Error", err)
        return
    }

    fmt.Println("Server certificate updated")
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_updateservercert.go) on GitHub\.

## Delete a Server Certificate<a name="iam-example-delete-server-certificate"></a>

In this example, you delete an existing server certificate\.

Create a new Go file named `iam_deleteservercert.go`\. You call the `DeleteServerCertificate` method of the IAM service object to change the name of the certificate\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
package main

import (
    "fmt"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/iam"
)
```

Set up a new IAM client\.

```
func main() {
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create a IAM service client.
    svc := iam.New(sess)
```

Call the method to delete the certificate, specifying the name of certificate\.

```
    _, err = svc.DeleteServerCertificate(&iam.DeleteServerCertificateInput{
        ServerCertificateName: aws.String("CERTIFICATE_NAME"),
    })
    if err != nil {
        fmt.Println("Error", err)
        return
    }

    fmt.Println("Server certificate deleted")
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_deleteservercert.go) on GitHub\.