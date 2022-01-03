# Listing User Docs<a name="wd-example-list-user-docs"></a>

The following example lists the documents for the user whose name is specified on the command line\. Choose `Copy` to save the code locally, or see the link to the complete example at the end of this topic\.

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

Get the user and organization ID from the command line\. If either is missing, print an error message and quit\.

```
userPtr := flag.String("u", "", "User for whom info is retrieved")
orgPtr := flag.String("o", "", "Your organization ID")

flag.Parse()

if *userPtr == "" || *orgPtr == "" {
    flag.PrintDefaults()
    os.Exit(1)
}
```

Create a session and Amazon WorkDocs client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

svc := workdocs.New(sess)
```

Get the root folder for the user\.

```
input := new(workdocs.DescribeUsersInput)
input.OrganizationId = orgPtr
input.Query = userPtr

result, err := svc.DescribeUsers(input)
if err != nil {
    fmt.Println("Error getting user info", err)
    return
}

var folderID = ""

if *result.TotalNumberOfUsers == 1 {
    for _, user := range result.Users {
        folderID = *user.RootFolderId
    }
```

Run the `DescribeFolderContents` method and display the name, size, and last modified information for each document\.

```
result, err := svc.DescribeFolderContents(&workdocs.DescribeFolderContentsInput{FolderId: &folderID})

if err != nil {
    fmt.Println("Error getting docs for user", err)
    return
}

fmt.Println(*userPtr + " docs:")
fmt.Println("")

for _, doc := range result.Documents {
    fmt.Println(*doc.LatestVersionMetadata.Name)
    fmt.Println("  Size:         ", *doc.LatestVersionMetadata.Size, "(bytes)")
    fmt.Println("  Last modified:", *doc.LatestVersionMetadata.ModifiedTimestamp)
    fmt.Println("")
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/workdocs/wd_list_user_docs.go) on GitHub\.