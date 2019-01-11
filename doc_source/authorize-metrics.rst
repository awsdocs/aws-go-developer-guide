.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _authorize_metrics:

########################################################
Authorize |CSM| to Collect and Send Metrics in the |sdk|
########################################################

To collect metrics from AWS SDKs using |CSM| for Enterprise Support,
Enterprise customers must create an |IAM| Role that gives |CW| agent permission
to gather data from their |EC2| instance or production environment.

Use the following |language| code sample or the AWS Console to create an
|IAM| Policy and Role for an |CW| agent to access |CSM| in your environment.

Learn more about using |CSM| with |sdk| in :doc:`setup-metrics`.

.. For more information about |CSM|, see |CW_IAM_CSM| in the *|CWlong| User Guide*.

.. _setup_access_permissions_sdk:

Set Up Access Permissions Using the |sdk|
=========================================

Create an |IAM| role for the instance that has permission for |EC2| Systems Manager and |CSM|.

First, create a policy using |CreatePolicy|.
Then create a role using |CreateRole|.
Finally, attach the policy you created to your new role with |AttachRolePolicy|.

.. replace with iam.go.create_csm_role once we release

.. code-block:: go

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

.. _setup_access_permissions_console:

Set Up Access Permissions by Using the |IAM| Console
====================================================

Alternatively, you can use the |IAM| console to create a role.

#. Go to the |IAM| console, and create a role to use |EC2|.

#. In the navigation pane, choose **Roles**.

#. Choose **Create Role**.

#. Choose **AWS Service**, and then **EC2**.

#. Choose **Next: Permissions**.

#. Under **Attach permissions policies**, choose **create policy**.

#. For **Service**, choose **Systems Manager**.
   For **Actions**, expand **Read**, and choose ``GetParameters``.
   For resources, specify your |CW| agent.

#. Add additional permission.

#. Select **Choose a service**, and then **Enter service manually**.
   For **Service**, enter ``sdkmetrics``.
   Select all ``sdkmetrics`` actions and all resources, and then choose **Review Policy**.

#. Name the **Role** ``AmazonSDKMetrics``, and add a description.

#. Choose **Create Role**.
