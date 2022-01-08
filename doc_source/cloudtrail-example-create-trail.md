# Creating a CloudTrail Trail<a name="cloudtrail-example-create-trail"></a>

This example uses the [CreateTrail](https://docs.aws.amazon.com/sdk-for-go/api/service/cloudtrail/#CloudTrail.CreateTrail) operation to create a CloudTrail trail in the `us-west-2` region\. It requires two inputs, the name of the trail and the name of the bucket in which CloudTrail stores information\. If the bucket does not have the proper policy, include the **\-p** flag to attach the correct policy to the bucket\.

Choose `Copy` to save the code locally\.

Create the file *create\_trail\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/cloudtrail"
    "github.com/aws/aws-sdk-go/service/s3"
    "github.com/aws/aws-sdk-go/service/sts"

    "encoding/json"
    "flag"
    "fmt"
)
```

Get the names of the trail and bucket, and whether to attach the policy to the bucket\. If either the trail name or bucket name is missing, display an error message and exit\.

```
trailNamePtr := flag.String("n", "", "The name of the trail")
bucketNamePtr := flag.String("b", "", "the name of the bucket to which the trails are uploaded")
addPolicyPtr := flag.Bool("p", false, "Whether to add the CloudTrail policy to the bucket")

flag.Parse()

if *trailNamePtr == "" || *bucketNamePtr == "" {
    fmt.Println("You must supply a trail name and bucket name.")
    return
}
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `.aws/credentials` in your home folder\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
```

If the **\-p** flag was specfied, add a policy to the bucket\.

```
if *addPolicyPtr {
    svc := sts.New(sess)
    input := &sts.GetCallerIdentityInput{}

    result, err := svc.GetCallerIdentity(input)
    if err != nil {
        fmt.Println("Got error snarfing caller identity:")
        fmt.Println(err.Error())
        return
    }

    accountID := aws.StringValue(result.Account)

    s3Policy := map[string]interface{}{
        "Version": "2012-10-17",
        "Statement": []map[string]interface{}{
            {
                "Sid":    "AWSCloudTrailAclCheck20150319",
                "Effect": "Allow",
                "Principal": map[string]interface{}{
                    "Service": "cloudtrail.amazonaws.com",
                },
                "Action":   "s3:GetBucketAcl",
                "Resource": "arn:aws:s3:::" + *bucketNamePtr,
            },
            {
                "Sid":    "AWSCloudTrailWrite20150319",
                "Effect": "Allow",
                "Principal": map[string]interface{}{
                    "Service": "cloudtrail.amazonaws.com",
                },
                "Action":   "s3:PutObject",
                "Resource": "arn:aws:s3:::" + *bucketNamePtr + "/AWSLogs/" + accountID + "/*",
                "Condition": map[string]interface{}{
                    "StringEquals": map[string]interface{}{
                        "s3:x-amz-acl": "bucket-owner-full-control",
                    },
                },
            },
        },
    }

    policy, err := json.Marshal(s3Policy)
    if err != nil {
        fmt.Println("Error marshalling request")
        return
    }

    // Create S3 service
    s3Svc := s3.New(sess)

    // Now set the policy on the bucket
    _, err = s3Svc.PutBucketPolicy(&s3.PutBucketPolicyInput{
        Bucket: aws.String(*bucketNamePtr),
        Policy: aws.String(string(policy)),
    })
    if err != nil {
        fmt.Print("Got error adding bucket policy:")
        fmt.Print(err.Error())
        return
    }

    fmt.Printf("Successfully set bucket %q's policy\n", *bucketNamePtr)
}
```

Create the CloudTrail client, the input for **CreateTrail**, and call **CreateTrail**\. If an error occurs, print the error and exit\. If no error occurs, print a success message\.

```
svc := cloudtrail.New(sess)

input := &cloudtrail.CreateTrailInput{
    Name:         aws.String(*trailNamePtr),
    S3BucketName: aws.String(*bucketNamePtr),
}

_, err := svc.CreateTrail(input)
if err != nil {
    fmt.Println("Got error calling CreateTrail:")
    fmt.Println(err.Error())
    return
}

fmt.Println("Created the trail", *trailNamePtr, "for bucket", *bucketNamePtr)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/cloudtrail/create_trail.go) on GitHub\.