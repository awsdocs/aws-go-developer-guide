# Deleting an Email Address in Amazon SES<a name="ses-example-delete-address"></a>

The following example demonstrates how to use the AWS SDK for Go to delete an Amazon SES email address\.

```
package main

import (
    "fmt"
    "os"
    
    //go get -u github.com/aws/aws-sdk-go
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/ses"
)

const (
    // Replace sender@example.com with your "From" address
    Sender = "sender@example.com"
    
    // Replace recipient@example.com with a "To" address
    Recipient = "recipient@example.com"
)

func main() {
    // Create a new session in the us-west-2 region
    // Replace us-west-2 with the AWS Region you're using for Amazon SES
    sess, err := session.NewSession(&aws.Config{
        Region:aws.String("us-west-2")},
    )

    if err != nil {
        fmt.Println("Got error creating SES session:")
        fmt.Println(err.Error())
        os.Exit(1)
    }
    
    // Create an SES session
    svc := ses.New(sess)

    // Remove email address
    _, delErr := svc.DeleteVerifiedEmailAddress(&ses.DeleteVerifiedEmailAddressInput{EmailAddress: aws.String(Recipient)})
    
    // Display error message if it occurs
    if delErr != nil {
        fmt.Println("Got error attempting to remove email address: " + Recipient)
        fmt.Println(delErr.Error())
      os.Exit(1)
    }

    // Display success message
    fmt.Println("Removed email address: " + Recipient)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/ses/ses_delete_address.go) on GitHub\.