# Listing Your AWS CodeBuild Project Builds<a name="cb-example-list-builds"></a>

The following example displays information about your AWS CodeBuild project builds, including the name of the project, when the build started, and how long each phase of the build took, in seconds\.

```
package main

import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/codebuild"
    "fmt"
    "os"
)

// Lists the CodeBuild builds for all projects in the region configured in the shared config
func main() {
    // Initialize a session in us-west-2 that the SDK will use to load
    // credentials from the shared credentials file ~/.aws/credentials.
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create CodeBuild service client
    svc := codebuild.New(sess)

    // Get the list of builds
    names, err := svc.ListBuilds(&codebuild.ListBuildsInput{SortOrder: aws.String("ASCENDING")})

    if err != nil {
        fmt.Println("Got error listing builds: ", err)
        os.Exit(1)
    }

    // Get information about each build
    builds, err := svc.BatchGetBuilds(&codebuild.BatchGetBuildsInput{Ids: names.Ids})

    if err != nil {
        fmt.Println("Got error getting builds: ", err)
        os.Exit(1)
    }

    for _, build := range builds.Builds {
        fmt.Printf("Project: %s\n", aws.StringValue(build.ProjectName))
        fmt.Printf("Phase:   %s\n", aws.StringValue(build.CurrentPhase))
        fmt.Printf("Status:  %s\n", aws.StringValue(build.BuildStatus))
        fmt.Println("")
    }
}
```

Choose `Copy` to save the code locally\. See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/codebuild/cb_list_builds.go) on GitHub\.