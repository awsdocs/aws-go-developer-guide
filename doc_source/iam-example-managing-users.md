# Managing IAM Users<a name="iam-example-managing-users"></a>

This Go example shows you how to create, update, view, and delete IAM users\. You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/iam) repository on GitHub\.

## Scenario<a name="iam-users-scenario"></a>

In this example, you use a series of Go routines to manage users in IAM\. The routines use the AWS SDK for Go IAM client methods that follow:
+  [CreateUser](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.CreateUser) 
+  [ListUsers](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.ListUsers) 
+  [UpdateUser](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.UpdateUser) 
+  [GetUser](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.GetUser) 
+  [DeleteUser](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.DeleteUser) 
+  [GetAccountAuthorizationDetails](https://docs.aws.amazon.com/sdk-for-go/api/service/iam/#IAM.GetAccountAuthorizationDetails) 

## Prerequisites<a name="iam-users-prerequisites"></a>
+ You have [set up](setting-up.md) and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with IAM users\. To learn more, see [IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) in the IAM User Guide\.

## Create a New IAM User<a name="iam-example-create-user"></a>

This code creates a new IAM user\.

Create a new Go file named `iam_createuser.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "fmt"
    "os"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/awserr"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/iam"
)
```

The code takes the new user name as an argument, and then calls `GetUser` with the user name\.

```
// Initialize a session in us-west-2 that the SDK will use to load
// credentials from the shared credentials file ~/.aws/credentials.
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Create a IAM service client.
svc := iam.New(sess)

_, err = svc.GetUser(&iam.GetUserInput{
    UserName: &os.Args[1],
})
```

If you receive a `NoSuchEntity` error, call `CreateUser` because the user doesn’t exist\.

```
if awserr, ok := err.(awserr.Error); ok && awserr.Code() == iam.ErrCodeNoSuchEntityException {
    result, err := svc.CreateUser(&iam.CreateUserInput{
        UserName: &os.Args[1],
    })

    if err != nil {
        fmt.Println("CreateUser Error", err)
        return
    }

    fmt.Println("Success", result)
} else {
    fmt.Println("GetUser Error", err)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_createuser.go) on GitHub\.

## List IAM Users in Your Account<a name="iam-example-list-user"></a>

You can get a list of the users in your account and print the list to the console\.

Create a new Go file named `iam_listusers.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "fmt"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/iam"
)
```

Set up a new IAM client\.

```
// Initialize a session in us-west-2 that the SDK will use to load
// credentials from the shared credentials file ~/.aws/credentials.
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Create a IAM service client.
svc := iam.New(sess)
```

Call `ListUsers` and print the results\.

```
result, err := svc.ListUsers(&iam.ListUsersInput{
    MaxItems: aws.Int64(10),
})

if err != nil {
    fmt.Println("Error", err)
    return
}

for i, user := range result.Users {
    if user == nil {
        continue
    }
    fmt.Printf("%d user %s created %v\n", i, *user.UserName, user.CreateDate)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_listusers.go) on GitHub\.

## Update a User’s Name<a name="iam-example-update-user"></a>

In this example, you change the name of an IAM user to a new value\.

Create a new Go file named `iam_updateuser.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
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
// Initialize a session in us-west-2 that the SDK will use to load
// credentials from the shared credentials file ~/.aws/credentials.
sess, err := session.NewSession(&aws.Config{
    Region: aws.String("us-west-2")},
)

// Create a IAM service client.
svc := iam.New(sess)
```

Call `UpdateUser`, passing in the original user name and the new name, and print the results\.

```
result, err := svc.UpdateUser(&iam.UpdateUserInput{
    UserName:    &os.Args[1],
    NewUserName: &os.Args[2],
})

if err != nil {
    fmt.Println("Error", err)
    return
}

fmt.Println("Success", result)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_updateuser.go) on GitHub\.

## Delete an IAM User<a name="iam-example-delete-user"></a>

In this example, you delete an IAM user\.

Create a new Go file named `iam_deleteuser.go`\. You must import the relevant Go and AWS SDK for Go packages by adding the following lines\.

```
import (
    "fmt"
    "os"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/awserr"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/iam"
)
```

Set up a new IAM client\.

```
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )
    svc := iam.New(sess)
```

Call `DeleteUser`, passing in the user name, and print the results\. If the user doesn’t exist, display an error\.

```
    _, err = svc.DeleteUser(&iam.DeleteUserInput{
        UserName: &os.Args[1],
    })

    // If the user does not exist than we will log an error.
    if awserr, ok := err.(awserr.Error); ok && awserr.Code() == iam.ErrCodeNoSuchEntityException {
        fmt.Printf("User %s does not exist\n", os.Args[1])
        return
    } else if err != nil {
        fmt.Println("Error", err)
        return
    }

    fmt.Printf("User %s has been deleted\n", os.Args[1])
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/iam/iam_deleteuser.go) on GitHub\.

## List the IAM Users who have Administrator Privileges<a name="iam-example-get-admins"></a>

In this example, you list the IAM users who have administrator privileges \(a policy or attached policy of the user or a group to which the user belongs has the name **AdministratorAccess**\.

Create a new Go file named `IamGetAdmins.go`\. Import the following packages\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/iam"

    "fmt"
    "os"
)
```

Create a method to determine whether a user has a policy that has administrator privileges\.

```
func UserPolicyHasAdmin(user *iam.UserDetail, admin string) bool {
    for _, policy := range user.UserPolicyList {
        if *policy.PolicyName == admin {
            return true
        }
    }

    return false
}
```

Create a method to determine whether a user has an attached policy that has administrator privileges\.

```
func AttachedUserPolicyHasAdmin(user *iam.UserDetail, admin string) bool {
    for _, policy := range user.AttachedManagedPolicies {
        if *policy.PolicyName == admin {
            return true
        }
    }

    return false
}
```

Create a method that determines whether a group has a policy that has adminstrator privileges\.

```
func GroupPolicyHasAdmin(svc *iam.IAM, group *iam.Group, admin string) bool {
    input := &iam.ListGroupPoliciesInput{
        GroupName: group.GroupName,
    }

    result, err := svc.ListGroupPolicies(input)
    if err != nil {
        fmt.Println("Got error calling ListGroupPolicies for group", group.GroupName)
    }

    // Wade through policies
    for _, policyName := range result.PolicyNames {
        if
        *policyName == admin {
            return true
        }
    }

    return false
}
```

Create a method that determines whether a group has an attached policy that has administrator privileges\.

```
func AttachedGroupPolicyHasAdmin(svc *iam.IAM, group *iam.Group, admin string) bool {
    input := &iam.ListAttachedGroupPoliciesInput{GroupName: group.GroupName}
    result, err := svc.ListAttachedGroupPolicies(input)
    if err != nil {
        fmt.Println("Got error getting attached group policies:")
        fmt.Println(err.Error())
        os.Exit(1)
    }

    for _, policy := range result.AttachedPolicies {
        if *policy.PolicyName == admin {
            return true
        }
    }

    return false
}
```

Create a method that determines whether any group to which the user belongs has administrator privileges\.

```
func UsersGroupsHaveAdmin(svc *iam.IAM, user *iam.UserDetail, admin string) bool {
    input := &iam.ListGroupsForUserInput{UserName: user.UserName}
    result, err := svc.ListGroupsForUser(input)
    if err != nil {
        fmt.Println("Got error getting groups for user:")
        fmt.Println(err.Error())
        os.Exit(1)
    }

    for _, group := range result.Groups {
        groupPolicyHasAdmin := GroupPolicyHasAdmin(svc, group, admin)

        if groupPolicyHasAdmin {
            return true
        }

        attachedGroupPolicyHasAdmin := AttachedGroupPolicyHasAdmin(svc, group, admin)

        if attachedGroupPolicyHasAdmin {
            return true
        }
    }

    return false
}
```

Create a method that determines whether a user has administrator privileges\.

```
func IsUserAdmin(svc *iam.IAM, user *iam.UserDetail, admin string) bool {
    // Check policy, attached policy, and groups (policy and attached policy)
    policyHasAdmin := UserPolicyHasAdmin(user, admin)
    if policyHasAdmin {
        return true
    }

    attachedPolicyHasAdmin := AttachedUserPolicyHasAdmin(user, admin)
    if attachedPolicyHasAdmin {
        return true
    }

    userGroupsHaveAdmin := UsersGroupsHaveAdmin(svc, user, admin)
    if userGroupsHaveAdmin {
        return true
    }

    return false
}
```

Create a main method with an IAM client in `us-west-2`\. Create variables to keep track of how many users we have and how many of those have administrator privileges\.

```
sess, err := session.NewSession()
if err != nil {
    fmt.Println("Got error creating new session")
    fmt.Println(err.Error())
    os.Exit(1)
}

svc := iam.New(sess, &aws.Config{Region: aws.String("us-west-2")})

numUsers := 0
numAdmins := 0
```

Create the input for and call `GetAccountAuthorizationDetails`\. If there is an error, print an error message and quit\.

```
user := "User"
input := &iam.GetAccountAuthorizationDetailsInput{Filter: []*string{&user}}
resp, err := svc.GetAccountAuthorizationDetails(input)
if err != nil {
    fmt.Println("Got error getting account details")
    fmt.Println(err.Error())
    os.Exit(1)
}
```

Loop through the users\. If a user has adminstrator privileges, print their name and increment the number of users who have adminstrator privileges\.

```
adminName := "AdministratorAccess"

// Wade through resulting users
for _, user := range resp.UserDetailList {
    numUsers += 1

    isAdmin := IsUserAdmin(svc, user, adminName)

    if isAdmin {
        fmt.Println(*user.UserName)
        numAdmins += 1
    }
}
```

If we did not get all of the users in the first call to `GetAccountAuthorizationDetails`, loop through the next set of users and determine which of those have adminstrator privileges\.

```
for *resp.IsTruncated {
    input := &iam.GetAccountAuthorizationDetailsInput{Filter: []*string{&user}, Marker: resp.Marker}
    resp, err = svc.GetAccountAuthorizationDetails(input)
    if err != nil {
        fmt.Println("Got error getting account details")
        fmt.Println(err.Error())
        os.Exit(1)
    }

    // Wade through resulting users
    for _, user := range resp.UserDetailList {
        numUsers += 1

        isAdmin := IsUserAdmin(svc, user, adminName)

        if isAdmin {
            fmt.Println(*user.UserName)
            numAdmins += 1
        }
    }
}
```

Finally, display the number of users who have administrator access\.

```
fmt.Println("")
fmt.Println("Found", numAdmins, "admin(s) out of", numUsers, "user(s).")
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/iam/IamListAdmins.go) on GitHub\.