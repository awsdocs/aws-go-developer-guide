# Verifying an Email Address in Amazon SES<a name="ses-example-send-verification"></a>

The following example demonstrates how to use the AWS SDK for Go to verify an Amazon SES email address\.

```
package main

import (
    "fmt"
    
    //go get -u github.com/aws/aws-sdk-go
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/ses"
    "github.com/aws/aws-sdk-go/aws/awserr"
)

const (
    // Replace sender@example.com with your "From" address. 
    // This address must be verified with Amazon SES.
    Sender = "sender@example.com"
    
    // Replace recipient@example.com with a "To" address. If your account 
    // is still in the sandbox, this address must be verified.
    Recipient = "recipient@example.com"
)

func main() {
    // Create a new session in the us-west-2 region.
    // Replace us-west-2 with the AWS Region you're using for Amazon SES.
    sess, err := session.NewSession(&aws.Config{
        Region:aws.String("us-west-2")},
    )

    // Create an SES session.
    svc := ses.New(sess)
    
    // Attempt to send the email.
    _, err = svc.VerifyEmailAddress(&ses.VerifyEmailAddressInput{EmailAddress: aws.String(Recipient)})
    
    // Display error messages if they occur.
    if err != nil {
        if aerr, ok := err.(awserr.Error); ok {
            switch aerr.Code() {
            case ses.ErrCodeMessageRejected:
                fmt.Println(ses.ErrCodeMessageRejected, aerr.Error())
            case ses.ErrCodeMailFromDomainNotVerifiedException:
                fmt.Println(ses.ErrCodeMailFromDomainNotVerifiedException, aerr.Error())
            case ses.ErrCodeConfigurationSetDoesNotExistException:
                fmt.Println(ses.ErrCodeConfigurationSetDoesNotExistException, aerr.Error())
            default:
                fmt.Println(aerr.Error())
            }
        } else {
            // Print the error, cast err to awserr.Error to get the Code and
            // Message from an error.
            fmt.Println(err.Error())
        }
    
        return
    }
    
    fmt.Println("Verification sent to address: " + Recipient)
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ses/ses_send_verification.go) on GitHub\.