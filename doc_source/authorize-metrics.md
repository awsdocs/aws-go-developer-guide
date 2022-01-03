# Authorize SDK Metrics to Collect and Send Metrics in the AWS SDK for Go<a name="authorize-metrics"></a>

To collect metrics from AWS SDKs using SDK Metrics for Enterprise Support, Enterprise customers must create an IAM Role that gives CloudWatch agent permission to gather data from their Amazon EC2 instance or production environment\.

Use the following Go code sample or the AWS Console to create an IAM Policy and Role for an CloudWatch agent to access SDK Metrics in your environment\.

Learn more about using SDK Metrics with AWS SDK for Go in [Set up SDK Metrics in the AWS SDK for Go](setup-metrics.md)\.

## Set Up Access Permissions Using the AWS SDK for Go<a name="setup-access-permissions-sdk"></a>

Create an IAM role for the instance that has permission for Amazon EC2 Systems Manager and SDK Metrics\.

First, create a policy using `CreatePolicy`\. Then create a role using `CreateRole`\. Finally, attach the policy you created to your new role with `AttachRolePolicy`\.

```
package main

import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/iam"

    "fmt"
    "encoding/json"
    "os"
)

/**
 * Creates a new managed policy for your AWS account.
 *
 * This code assumes that you have already set up AWS credentials. See
 * https://docs.aws.amazon.com/sdk-for-go/v1/developer-guide/configuring-sdk.html#specifying-credentials
 */

func main() {
    // Default name for policy, role policy.
    RoleName := "AmazonCSM"

    // Override name if provided
    if len(os.Args) == 2 {
        RoleName = os.Args[1]
    }

    Description := "An instance role that has permission for AWS Systems Manager and SDK Metric Monitoring."

    // Initialize a session that the SDK uses to
    // load credentials from ~/.aws/credentials
    // and region from ~/.aws/config.
    sess := session.Must(session.NewSessionWithOptions(session.Options{
        SharedConfigState: session.SharedConfigEnable,
    }))

    // Create new IAM client
    svc := iam.New(sess)

    AmazonCSMPolicy := map[string]interface{}{
        "Version": "2012-10-17",
        "Statement": []map[string]interface{}{
            {
                "Effect": "Allow",
                "Action": "sdkmetrics:*",
                "Resource": "*",
            },
            {
                "Effect": "Allow",
                "Action": "ssm:GetParameter",
                "Resource": "arn:aws:ssm:*:*:parameter/AmazonCSM*",
            },
        },
    }

    policy, err := json.Marshal(AmazonCSMPolicy)
    if err != nil {
        fmt.Println("Got error marshalling policy")
        fmt.Println(err.Error())
        os.Exit(0)
    }

    // Create policy
    policyResponse, err := svc.CreatePolicy(&iam.CreatePolicyInput{
        PolicyDocument: aws.String(string(policy)),
        PolicyName: aws.String(RoleName + "policy"),
    })
    if err != nil {
        fmt.Println("Got error creating policy:")
        fmt.Println(err.Error())
        os.Exit(1)
    }

    // Create role policy
    RolePolicyJSON := map[string]interface{}{
        "Version": "2012-10-17",
        "Statement": []map[string]interface{}{
            {
                "Effect": "Allow",
                "Principal": map[string]interface{}{
                    "Service": "ec2.amazonaws.com",
                },
                "Action": "sts:AssumeRole",
            },
        },
    }

    RolePolicy, err := json.Marshal(RolePolicyJSON)
    if err != nil {
        fmt.Println("Got error marshalling role policy:")
        fmt.Println(err.Error())
        os.Exit(0)
    }

    // Create the inputs for the role
    input := &iam.CreateRoleInput{
        AssumeRolePolicyDocument: aws.String(string(RolePolicy)),
        Description: aws.String(Description),
        RoleName: aws.String(RoleName),
    }

    _, err = svc.CreateRole(input)
    if err != nil {
        fmt.Println("Got error creating role:")
        fmt.Println(err.Error())
        os.Exit(0)
    }

    // Attach policy to role
    _, err = svc.AttachRolePolicy(&iam.AttachRolePolicyInput{
        PolicyArn: aws.String(*policyResponse.Policy.Arn),
        RoleName: aws.String(RoleName),
    })
    if err != nil {
        fmt.Println("Got error attaching policy to role:")
        fmt.Println(err.Error())
        os.Exit(0)
    }

    fmt.Println("Successfully created role: " + RoleName)
}
```

## Set Up Access Permissions by Using the IAM Console<a name="setup-access-permissions-console"></a>

Alternatively, you can use the IAM console to create a role\.

1. Go to the IAM console, and create a role to use Amazon EC2\.

1. In the navigation pane, choose **Roles**\.

1. Choose **Create Role**\.

1. Choose **AWS Service**, and then **EC2**\.

1. Choose **Next: Permissions**\.

1. Under **Attach permissions policies**, choose **create policy**\.

1. For **Service**, choose **Systems Manager**\. For **Actions**, expand **Read**, and choose `GetParameters`\. For resources, specify your CloudWatch agent\.

1. Add additional permission\.

1. Select **Choose a service**, and then **Enter service manually**\. For **Service**, enter `sdkmetrics`\. Select all `sdkmetrics` actions and all resources, and then choose **Review Policy**\.

1. Name the **Role** `AmazonSDKMetrics`, and add a description\.

1. Choose **Create Role**\.