# Listing Users<a name="wd-example-list-users"></a>

The following example lists the names of all users, or lists additional details about a user if a user name is specified on the command line\. Choose `Copy` to save the code locally, or see the link to the complete example at the end of this topic\.

Import the following Go packages\.

```
import (
    "os"

    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/workdocs"

    "flag"
    "fmt"
)
```

Get an optional user name and organization ID from the command line\. If the organization ID is missing, print an error message and quit\.

```
orgPtr := flag.String("o", "", "The ID of your organization")
userPtr := flag.String("u", "", "User for whom info is retrieved")

flag.Parse()

if *orgPtr == "" {
    fmt.Println("You must supply the organization ID")
    flag.PrintDefaults()
    os.Exit(1)
}
```

Create input for the `DescribeUsers` method\.

```
input := new(workdocs.DescribeUsersInput)
input.OrganizationId = orgPtr

// Show all users if we don't get a user name
if *userPtr == "" {
    fmt.Println("Getting info about all users")
} else {
    fmt.Println("Getting info about user " + *userPtr)
    input.Query = userPtr
}
```

Create a session and Amazon WorkDocs client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

svc := workdocs.New(sess)
```

Call `DescribeUsers` and display the information for the user or all users\.

```
result, err := svc.DescribeUsers(input)
if err != nil {
    fmt.Println("Error getting user info", err)
    return
}

if *userPtr == "" {
    fmt.Println("Found", *result.TotalNumberOfUsers, "users")
    fmt.Println("")
}

for _, user := range result.Users {
    fmt.Println("Username:   " + *user.Username)

    if *userPtr != "" {
        fmt.Println("Firstname:  " + *user.GivenName)
        fmt.Println("Lastname:   " + *user.Surname)
        fmt.Println("Email:      " + *user.EmailAddress)
        fmt.Println("Root folder " + *user.RootFolderId)
    }

    fmt.Println("")
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/workdocs/wd_list_users.go) on GitHub\.