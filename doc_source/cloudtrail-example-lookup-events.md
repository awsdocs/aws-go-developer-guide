# Listing CloudTrail Trail Events<a name="cloudtrail-example-lookup-events"></a>

This example uses the [LookupEvents](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudtrail/#CloudTrail.LookupEvents) operation to list the CloudTrail trail events in the `us-west-2` region\.

Choose `Copy` to save the code locally\.

Create the file *lookup\_events\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/cloudtrail"

    "flag"
    "fmt"
    "time"
)
```

Get the name of the trail\. If the trail name is missing, display an error message and exit\.

```
trailNamePtr := flag.String("n", "", "The name of the trail")

flag.Parse()

if *trailNamePtr == "" {
    fmt.Println("You must supply a trail name")
    return
}
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `.aws/credentials` in your home folder, and create a new service client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

Create the CloudTrail client, and the input for and call **LookupEvents**\. If an error occurs, print the error and exit\. If no error occurs, loop through the events, printing information about each event\.

```
svc := cloudtrail.New(sess)

input := &cloudtrail.LookupEventsInput{EndTime: aws.Time(time.Now())}

resp, err := svc.LookupEvents(input)
if err != nil {
    fmt.Println("Got error calling CreateTrail:")
    fmt.Println(err.Error())
    return
}

fmt.Println("Found", len(resp.Events), "events before now")
fmt.Println("")

for _, event := range resp.Events {
    fmt.Println("Event:")
    fmt.Println(aws.StringValue(event.CloudTrailEvent))
    fmt.Println("")
    fmt.Println("Name    ", aws.StringValue(event.EventName))
    fmt.Println("ID:     ", aws.StringValue(event.EventId))
    fmt.Println("Time:   ", aws.TimeValue(event.EventTime))
    fmt.Println("User:   ", aws.StringValue(event.Username))

    fmt.Println("Resources:")

    for _, resource := range event.Resources {
        fmt.Println("  Name:", aws.StringValue(resource.ResourceName))
        fmt.Println("  Type:", aws.StringValue(resource.ResourceType))
    }

    fmt.Println("")
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/cloudtrail/lookup_events.go) on GitHub\.