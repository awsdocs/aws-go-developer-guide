# Working with IAM Policies<a name="iam-example-policies"></a>

This Go example shows you how to create, get, attach, and detach IAM policies\. You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/iam) repository on GitHub\.

## Scenario<a name="iam-policies-scenario"></a>

You grant permissions to a user by creating a policy, which is a document that lists the actions that a user can perform and the resources those actions can affect\. Any actions or resources that are not explicitly allowed are denied by default\. Policies can be created and attached to users, groups of users, roles assumed by users, and resources\.

In this example, you use a series of Go routines to manage policies in IAM\. The routines use the AWS SDK for Go IAM client methods that follow:
+  [CreatePolicy](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.CreatePolicy) 
+  [GetPolicy](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.GetPolicy) 
+  [ListAttachedRolePolicies](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.ListAttachedRolePolicies) 
+  [AttachRolePolicy](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.AttachRolePolicy) 
+  [DetachRolePolicy](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.DetachRolePolicy) 

## Prerequisites<a name="iam-policies-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with IAM policies\. To learn more, see [Overview of Access Management: Permissions and Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_access-management.html) in the IAM User Guide\.

## Create an IAM Policy<a name="iam-example-create-policy"></a>

This code creates a new IAM Policy\. Create a new Go file named `iam_createpolicy.go`\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
package main

import (
    "encoding/json"
    "fmt"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/iam"
)
```

Define two structs\. The first is the definition of the policies to upload to IAM\.

```
type PolicyDocument struct {
    Version   string
    Statement []StatementEntry
}
```

The second dictates what this policy will allow or disallow\.

```
type StatementEntry struct {
    Effect   string
    Action   []string
    Resource string
}
```

Set up the session and IAM client\.

```
func main() {
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create a IAM service client.
    svc := iam.New(sess)
```

Build the policy document using the structures defined earlier\.

```
    policy := PolicyDocument{
        Version: "2012-10-17",
        Statement: []StatementEntry{
            StatementEntry{
                Effect: "Allow",
                Action: []string{
                    "logs:CreateLogGroup", // Allow for creating log groups
                },
                Resource: "RESOURCE ARN FOR logs:*",
            },
            StatementEntry{
                Effect: "Allow",
                // Allows for DeleteItem, GetItem, PutItem, Scan, and UpdateItem
                Action: []string{
                    "dynamodb:DeleteItem",
                    "dynamodb:GetItem",
                    "dynamodb:PutItem",
                    "dynamodb:Scan",
                    "dynamodb:UpdateItem",
                },
                Resource: "RESOURCE ARN FOR dynamodb:*",
            },
        },
    }
```

Marshal the policy to JSON and pass to `CreatePolicy`\.

```
    b, err := json.Marshal(&policy)
    if err != nil {
        fmt.Println("Error marshaling policy", err)
        return
    }

    result, err := svc.CreatePolicy(&iam.CreatePolicyInput{
        PolicyDocument: aws.String(string(b)),
        PolicyName:     aws.String("myDynamodbPolicy"),
    })

    if err != nil {
        fmt.Println("Error", err)
        return
    }

    fmt.Println("New policy", result)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_createpolicy.go) on GitHub\.

## Get an IAM Policy<a name="iam-example-get-policy"></a>

In this example, you retrieve an existing policy from IAM\. Create a new Go file named `iam_getpolicy.go`\.

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
    // Initialize a session in us-west-2 that the SDK will use to load
    // credentials from the shared credentials file ~/.aws/credentials.
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create a IAM service client.
    svc := iam.New(sess)
```

Call `GetPolicy`, passing in the ARN for the policy \(which is hard coded in this example\), and print the results\.

```
    arn := "arn:aws:iam::aws:policy/AWSLambdaExecute"
    result, err := svc.GetPolicy(&iam.GetPolicyInput{
        PolicyArn: &arn,
    })

    if err != nil {
        fmt.Println("Error", err)
        return
    }

    fmt.Printf("%s - %s\n", arn, *result.Policy.Description)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_getpolicy.go) on GitHub\.

## Attach a Managed Role Policy<a name="iam-example-attach-managed-role-policy"></a>

In this example, you attach an IAM managed role policy\. Create a new Go file named `iam_attachuserpolicy.go`\. You’ll call the `ListAttachedRolePolicies` method of the IAM service object, which returns an array of managed policies\.

Then, you’ll check the array members to see if the policy you want to attach to the role is already attached\. If the policy isn’t attached, you’ll call the `AttachRolePolicy` method to attach it\.

You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

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

Set up a new IAM client\.

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

Declare variables to hold the name and ARN of the policy\.

```
    var pageErr error
    policyName := "AmazonDynamoDBFullAccess"
    policyArn := "arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess"
```

Paginate through all the role policies\. If your role exists on any role policy, you set the `pageErr` and return `false`, stopping the pagination\.

```
    err = svc.ListAttachedRolePoliciesPages(
        &iam.ListAttachedRolePoliciesInput{
            RoleName: &os.Args[1],
        },
        func(page *iam.ListAttachedRolePoliciesOutput, lastPage bool) bool {
            if page != nil && len(page.AttachedPolicies) > 0 {
                for _, policy := range page.AttachedPolicies {
                    if *policy.PolicyName == policyName {
                        pageErr = fmt.Errorf("%s is already attached to this role", policyName)
                        return false
                    }
                }
                // We should keep paginating because we did not find our role
                return true
            }
            return false
        },
    )
```

If your role policy is not attached already, call `AttachRolePolicy`\.

```
    if pageErr != nil {
        fmt.Println("Error", pageErr)
        return
    }

    if err != nil {
        fmt.Println("Error", err)
        return
    }

    _, err = svc.AttachRolePolicy(&iam.AttachRolePolicyInput{
        PolicyArn: &policyArn,
        RoleName:  &os.Args[1],
    })

    if err != nil {
        fmt.Println("Unable to attach role policy to role")
        return
    }
    fmt.Println("Role attached successfully")
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_attachuserpolicy.go) on GitHub\.

## Detach a Managed Role Policy<a name="iam-example-detach-managed-role-policy"></a>

In this example, you detach a role policy\. Once again, you call the `ListAttachedRolePolicies` method of the IAM service object, which returns an array of managed policies\.

Then, check the array members to see if the policy you want to detach from the role is attached\. If the policy is attached, call the `DetachRolePolicy` method to detach it\.

Create a new Go file named `iam_detachuserpolicy.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

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

Set up a new IAM client\.

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

Declare variables to hold the name and ARN of the policy\.

```
    foundPolicy := false
    policyName := "AmazonDynamoDBFullAccess"
    policyArn := "arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess"
```

Paginate through all the role policies\. If the role exists on any role policy, you stop iterating and detach the role\.

```
    err = svc.ListAttachedRolePoliciesPages(
        &iam.ListAttachedRolePoliciesInput{
            RoleName: &os.Args[1],
        },
        func(page *iam.ListAttachedRolePoliciesOutput, lastPage bool) bool {
            if page != nil && len(page.AttachedPolicies) > 0 {
                for _, policy := range page.AttachedPolicies {
                    if *policy.PolicyName == policyName {
                        foundPolicy = true
                        return false
                    }
                }
                return true
            }
            return false
        },
    )

    if err != nil {
        fmt.Println("Error", err)
        return
    }

    if !foundPolicy {
        fmt.Println("Policy was not attached to role")
        return
    }

    _, err = svc.DetachRolePolicy(&iam.DetachRolePolicyInput{
        PolicyArn: &policyArn,
        RoleName:  &os.Args[1],
    })

    if err != nil {
        fmt.Println("Unable to detach role policy to role")
        return
    }
    fmt.Println("Role detached successfully")
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_detachuserpolicy.go) on GitHub\.