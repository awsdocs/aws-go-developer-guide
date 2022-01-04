# Listing Valid Amazon SES Email Addresses<a name="ses-example-list-emails"></a>

The following example demonstrates how to use the AWS SDK for Go to list the valid Amazon SES email addresses\.

```
package main

import (
    "fmt"
    "os"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/ses"
)

func main() {
    // Initialize a session in us-west-2 that the SDK will use to load
    // credentials from the shared credentials file ~/.aws/credentials.
    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-west-2")},
    )

    // Create SES service client
    svc := ses.New(sess)

    result, err := svc.ListIdentities(&ses.ListIdentitiesInput{IdentityType: aws.String("EmailAddress")})

    if err != nil {
        fmt.Println(err)
        os.Exit(1)
    }

    for _, email := range result.Identities {
        var e = []*string{email}

        verified, err := svc.GetIdentityVerificationAttributes(&ses.GetIdentityVerificationAttributesInput{Identities: e})

        if err != nil {
            fmt.Println(err)
            os.Exit(1)
        }

        for _, va := range verified.VerificationAttributes {
            if *va.VerificationStatus == "Success" {
                fmt.Println(*email)
            }
        }
    }
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/ses/ses_list_emails.go) on GitHub\.