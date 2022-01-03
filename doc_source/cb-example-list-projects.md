# Getting Information about All AWS CodeBuild Projects<a name="cb-example-list-projects"></a>

The following example lists the names of up to 100 of your AWS CodeBuild projects\.

```
package main

import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/codebuild"
    "fmt"
    "os"
)

// Lists a CodeBuild projects in the region configured in the shared config
func main() {
    // Initialize a session in us-west-2 that the SDK will use to load
    // credentials from the shared credentials file ~/.aws/credentials.
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create CodeBuild service client
    svc := codebuild.New(sess)

    // Get the list of projects
    result, err := svc.ListProjects(
        &codebuild.ListProjectsInput{
            SortBy:    aws.String("NAME"),
            SortOrder: aws.String("ASCENDING", )})

    if err != nil {
        fmt.Println("Got error listing projects: ", err)
        os.Exit(1)
    }

    for _, p := range result.Projects {
        fmt.Println(*p)
    }
}
```

Choose `Copy` to save the code locally\. See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/codebuild/cb_list_projects.go) on GitHub\.