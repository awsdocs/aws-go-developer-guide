# Deleting a CloudTrail Trail<a name="cloudtrail-example-delete-trail"></a>

This example uses the [DeleteTrail](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudtrail/#CloudTrail.DeleteTrail) operation to delete a CloudTrail trail in the `us-west-2` region\. It requires one input, the name of the trail\.

Choose `Copy` to save the code locally\.

Create the file *delete\_trail\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/cloudtrail"

    "flag"
    "fmt"
)
```

Get the name of the trail\. If the trail name is missing, display an error message and exit\.

```
trailNamePtr := flag.String("n", "", "The name of the trail to delete")

flag.Parse()

if *trailNamePtr == "" {
    fmt.Println("You must supply a trail name")
    return
}
```

Initialize the session that the SDK uses to load credentials from the shared credentials file *\.aws/credentials* in your home folder and create the client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

Create a CloudTrail client and call **DeleteTrail** with the trail name\. If an error occurs, print the error and exit\. If no error occurs, print a success message\.

```
svc := cloudtrail.New(sess)

_, err := svc.DeleteTrail(&cloudtrail.DeleteTrailInput{Name: aws.String(*trailNamePtr)})
if err != nil {
    fmt.Println("Got error calling CreateTrail:")
    fmt.Println(err.Error())
    return
}

fmt.Println("Successfully deleted trail", *trailNamePtr)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/cloudtrail/delete_trail.go) on GitHub\.