# Managing IAM Access Keys<a name="iam-example-managing-access-keys"></a>

This Go example shows you how to create, modify, view, or rotate IAM access keys\. You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/iam) repository on GitHub\.

## Scenario<a name="iam-access-keys-scenario"></a>

Users need their own access keys to make programmatic calls to the AWS SDK for Go\. To fill this need, you can create, modify, view, or rotate access keys \(access key IDs and secret access keys\) for IAM users\. By default, when you create an access key its status is Active, which means the user can use the access key for API calls\.

In this example, you use a series of Go routines to manage access keys in IAM\. The routines use the AWS SDK for Go IAM client methods that follow:
+  [CreateAccessKey](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.CreateAccessKey) 
+  [ListAccessKeys](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.ListAccessKeys) 
+  [GetAccessKeyLastUsed](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.GetAccessKeyLastUsed) 
+  [UpdateAccessKey](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.UpdateAccessKey) 
+  [DeleteAccessKey](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.DeleteAccessKey) 

## Prerequisites<a name="iam-access-keys-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with IAM access keys\. To learn more, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the IAM User Guide\.

## Create a New IAM Access Key<a name="iam-example-create-access-key"></a>

This code creates a new IAM access key for the IAM user named IAM\_USER\_NAME\.

Create a new Go file named `iam_createaccesskey.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
package main

import (
    "fmt"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/iam"
)
```

Set up the session\.

```
func main() {
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create a IAM service client.
    svc := iam.New(sess)
```

Call `CreateAccessKey` and print the results\.

```
    result, err := svc.CreateAccessKey(&iam.CreateAccessKeyInput{
        UserName: aws.String("IAM_USER_NAME"),
    })

    if err != nil {
        fmt.Println("Error", err)
        return
    }

    fmt.Println("Success", *result.AccessKey)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_createaccesskey.go) on GitHub\.

## List a User’s Access Keys<a name="iam-example-list-access-keys"></a>

In this example, you get a list of the access keys for a user and print the list to the console\.

Create a new Go file named `iam_listaccesskeys.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

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

Call `ListAccessKeys` and print the results\.

```
    result, err := svc.ListAccessKeys(&iam.ListAccessKeysInput{
        MaxItems: aws.Int64(5),
        UserName: aws.String("IAM_USER_NAME"),
    })

    if err != nil {
        fmt.Println("Error", err)
        return
    }

    fmt.Println("Success", result)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_listaccesskeys.go) on GitHub\.

## Get the Last Use for an Access Key<a name="iam-example-last-use"></a>

In this example, you find out when an access key was last used\.

Create a new Go file named `iam_accesskeylastused.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

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

Call `GetAccessKeyLastUsed`, passing in the access key ID, and print the results\.

```
    result, err := svc.GetAccessKeyLastUsed(&iam.GetAccessKeyLastUsedInput{
        AccessKeyId: aws.String("ACCESS_KEY_ID"),
    })

    if err != nil {
        fmt.Println("Error", err)
        return
    }

    fmt.Println("Success", *result.AccessKeyLastUsed)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_accesskeylastused.go) on GitHub\.

## Update Access Key Status<a name="iam-example-update-access-key"></a>

In this example, you delete an IAM user\.

Create a new Go file with the name `iam_updateaccesskey.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

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

Call `UpdateAccessKey`, passing in the access key ID, status \(making it active in this case\), and user name\.

```
    _, err = svc.UpdateAccessKey(&iam.UpdateAccessKeyInput{
        AccessKeyId: aws.String("ACCESS_KEY_ID"),
        Status:      aws.String(iam.StatusTypeActive),
        UserName:    aws.String("USER_NAME"),
    })

    if err != nil {
        fmt.Println("Error", err)
        return
    }

    fmt.Println("Access Key updated")
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_updateaccesskey.go) on GitHub\.

## Delete an Access Key<a name="iam-example-delete-access-key"></a>

In this example, you delete an access key\.

Create a new Go file named `iam_deleteaccesskey.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

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

Call `DeleteAccessKey`, passing in the access key ID and user name\.

```
    result, err := svc.DeleteAccessKey(&iam.DeleteAccessKeyInput{
        AccessKeyId: aws.String("ACCESS_KEY_ID"),
        UserName:    aws.String("USER_NAME"),
    })

    if err != nil {
        fmt.Println("Error", err)
        return
    }

    fmt.Println("Success", result)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_deleteaccesskey.go) on GitHub\.