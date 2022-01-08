# Listing the CloudTrail Trails<a name="cloudtrail-example-describe-trails"></a>

This example uses the [DescribeTrails](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudtrail/#CloudTrail.DescribeTrails) operation to list the names of the CloudTrail trails and the bucket in which CloudTrail stores information in the `us-west-2` region\.

Choose `Copy` to save the code locally\.

Create the file *describe\_trails\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "fmt"

    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/cloudtrail"
)
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `.aws/credentials` in your home folder, and create a new service client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

// Create CloudTrail client
svc := cloudtrail.New(sess)
```

Call **DescribeTrails**\. If an error occurs, print the error and exit\. If no error occurs, loop through the trails, printing the name of each trail and the bucket\.

```
resp, err := svc.DescribeTrails(&cloudtrail.DescribeTrailsInput{TrailNameList: nil})
if err != nil {
    fmt.Println("Got error calling CreateTrail:")
    fmt.Println(err.Error())
    return
}

fmt.Println("Found", len(resp.TrailList), "trail(s)")
fmt.Println("")

for _, trail := range resp.TrailList {
    fmt.Println("Trail name:  " + *trail.Name)
    fmt.Println("Bucket name: " + *trail.S3BucketName)
    fmt.Println("")
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/cloudtrail/describe_trails.go) on GitHub\.