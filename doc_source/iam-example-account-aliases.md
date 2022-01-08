# Managing IAM Account Aliases<a name="iam-example-account-aliases"></a>

This Go example shows you how to create, list, and delete IAM account aliases\. You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/iam) repository on GitHub\.

## Scenario<a name="iam-aliases-scenario"></a>

You can use a series of Go routines to manage aliases in IAM\. The routines use the AWS SDK for Go IAM client methods that follow:
+  [CreateAccountAlias](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.CreateAccountAlias) 
+  [ListAccountAliases](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.ListAccountAliases) 
+  [DeleteAccountAlias](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.DeleteAccountAlias) 

## Prerequisites<a name="iam-aliases-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with IAM account aliases\. To learn more, see [Your AWS account ID and Its Alias](https://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html) in the IAM User Guide\.

## Create a New IAM Account Alias<a name="iam-example-create-alias"></a>

This code creates a new IAM user\.

Create a new Go file named `iam_createaccountalias.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
package main

import (
    "fmt"
    "os"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/iam"
)
```

Set up a session and an IAM client\.

```
func main() {
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create a IAM service client.
    svc := iam.New(sess)
```

The code takes the new alias as an argument, and then calls `CreateAccountAlias` with the alias name\.

```
    _, err = svc.CreateAccountAlias(&iam.CreateAccountAliasInput{
        AccountAlias: &os.Args[1],
    })

    if err != nil {
        fmt.Println("Error", err)
        return
    }

    fmt.Printf("Account alias %s has been created\n", os.Args[1])
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_createaccountalias.go) on GitHub\.

## List IAM Account Aliases<a name="iam-example-list-aliases"></a>

This code lists the aliases for your IAM account\.

Create a new Go file named `iam_listaccountaliases.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
package main

import (
    "fmt"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/iam"
)
```

Set up a session and an IAM client\.

```
func main() {
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create a IAM service client.
    svc := iam.New(sess)
```

The code calls `ListAccountAliases`, specifying to return a maximum of 10 items\.

```
    result, err := svc.ListAccountAliases(&iam.ListAccountAliasesInput{
        MaxItems: aws.Int64(10),
    })

    if err != nil {
        fmt.Println("Error", err)
        return
    }

    for i, alias := range result.AccountAliases {
        if alias == nil {
            continue
        }
        fmt.Printf("Alias %d: %s\n", i, *alias)
    }
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_listaccountaliases.go) on GitHub\.

## Delete an IAM Account Alias<a name="iam-example-delete-aliases"></a>

This code deletes a specified IAM account alias\.

Create a new Go file with the name `iam_deleteaccountalias.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
package main

import (
    "fmt"
    "os"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/iam"
)
```

Set up a session and an IAM client\.

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

The code calls `ListAccountAliases`, specifying to return a maximum of 10 items\.

```
    _, err = svc.DeleteAccountAlias(&iam.DeleteAccountAliasInput{
        AccountAlias: &os.Args[1],
    })

    if err != nil {
        fmt.Println("Error", err)
        return
    }

    fmt.Printf("Alias %s has been deleted\n", os.Args[1])
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_deleteaccountalias.go) on GitHub\.