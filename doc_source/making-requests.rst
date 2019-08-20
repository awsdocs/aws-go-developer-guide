.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

####################################
Using the |sdk-go| with AWS Services
####################################

.. meta::
   :description: Construct service clients and make operation calls to send requests to AWS services.
   :keywords: clients, service clients

To make calls to an AWS service, you must first construct a service
client instance with a session. A service client provides low-level
access to every API action for that service. For example, you create an
|S3| service client to make calls to |S3|.

When you call service operations, you pass in input parameters as a
struct. A successful call usually results in an output struct that you
can use. For example, after you successfully call an |S3| create bucket
action, the action returns an output struct with the bucket's location.

For the list of service clients, including their methods and parameters,
see the |sdk-go-api|_.

.. _constructing-a-service:

Constructing a Service
======================

To construct a service client instance, use the :sdk-go-api-deep:`NewSession() <aws/session/#NewSession>`
function. The following example creates an |S3| service client.

.. code:: go

    sess, err := session.NewSession()
    if err != nil {
        fmt.Println("Error creating session ", err)
        return
    }
    svc := s3.New(sess)

After you have a service client instance, you can use it to call service
operations. For more information about configurations, see :doc:`configuring-sdk`.

When you create a service client, you can pass in custom configurations
so that you don't need to create a session for each configuration. The
SDK merges the two configurations, overriding session values with your
custom configuration. For example, in the following snippet, the |S3|
client uses the ``mySession`` session but overrides the ``Region`` field
with a custom value (``us-west-2``):

.. code:: go

    svc := s3.New(mySession, aws.NewConfig().WithRegion("us-west-2"))

.. _tag-service-resources:

Tagging Service Resources
=========================

You can tag service resources,
such as |S3| buckets,
so that you can determine the costs of your service resources
at whatever level of granularity you require.

The following example shows how to tag the |S3| bucket
:code:`MyBucket` with :code:`Cost Center` tag with the value :code:`123456`
and :code:`Stack` tag with the value :code:`MyTestStack`.

.. literalinclude:: extending.go.add_tags.txt
   :dedent: 0
   :language: go

Note that if a tag of the same name already exists,
its value is overwritten by the new value.

.. _get-http-request-response:

Getting the HTTP Request and Response with Each Service Call
============================================================

You can direct the |sdk-go| to display the HTTP request
and response it sends and receives for each call by
including a configuration option when constructing the service client.

The following example uses the |DDB| **ListTables** operation
to illustrate how to add a custom header to a service call.

.. literalinclude:: example_code/extending_sdk/addHeader.go
   :lines: 15-
   :dedent: 0
   :language: go

If you run this program, the output should be similar to the following,
where **ACCESS-KEY** is the access key of the user and
**TABLE-1**, through **TABLE-N** are the names of the tables.

.. literalinclude:: example_code/extending_sdk/tableList.txt
   :lines: 1-

.. _service-operation-calls:

Service Operation Calls
=======================

You can call a service operation directly or with its request form. When
you call a service operation, the SDK synchronously validates the input,
builds the request, signs it with your credentials, sends it to AWS, and
then gets a response or an error. In most cases, you can call service
operations directly.

.. _calling-operations:

Calling Operations
------------------

Calling the operation will sync as the request is built, signed, sent,
and the response is received. If an error occurs during the operation,
it will be returned. The output or resulting structure won't be valid.

For example, to call the |S3| GET Object API, use the |S3|
service client instance and call its :sdk-go-api-deep:`GetObject <service/s3/#S3.GetObject>`
method:

.. code:: go

    result, err := s3Svc.GetObject(&s3.GetObjectInput{...})
    // result is a *s3.GetObjectOutput struct pointer
    // err is a error which can be cast to awserr.Error.

.. _passing_parameters_to_a_service_operation:

Passing Parameters to a Service Operation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When calling an operation on a service, you pass in input parameters as
option values, similar to passing in a configuration. For example, to
retrieve an object, you must specify a bucket and the object's key by
passing in the following parameters to the ``GetObject`` method:

.. code:: go

    svc := s3.New(session.New())
    svc.GetObject(&s3.GetObjectInput{
        Bucket: aws.String("bucketName"),
        Key:    aws.String("keyName"),
    })

Each service operation has an associated input struct and, usually, an
output struct. The structs follow the naming pattern
*OperationName*\ ``Input`` and *OperationName*\ ``Output``.

For more information about the parameters of each method, see the
service client documentation in the |sdk-go-api|_.

.. _calling-operations-with-the-request-form:

Calling Operations with the Request Form
----------------------------------------

Calling the request form of a service operation, which follows the
naming pattern *OperationName*\ ``Request``, provides a simple way to
control when a request is built, signed, and sent. Calling the request
form immediately returns a request object. The request object output is
a struct pointer that is not valid until the request is sent and
returned successfully.

Calling the request form can be useful when you want to construct a
number of pre-signed requests, such as pre-signed |S3| URLs. You
can also use the request form to modify how the SDK sends a request.

The following example calls the request form of the ``GetObject``
method. The :sdk-go-api-deep:`Send <aws/request/#Request.Send>` method signs
the request before sending it.

.. code:: go

    req, result := s3Svc.GetObjectRequest(&s3.GetObjectInput{...})
    // result is a *s3.GetObjectOutput struct pointer, not populated until req.Send() returns
    // req is a *aws.Request struct pointer. Used to Send request.
    if err := req.Send(); err != nil {
        // process error
        return
    }
    // Process result

.. _handling-operation-response-body:

Handling Operation Response Body
--------------------------------

Some API operations return a response struct that contain a ``Body``
field that is an ``io.ReadCloser``. If you're making requests with
these operations, always be sure to call :code:`Close` on the field.

.. code:: go

    resp, err := s3svc.GetObject(&s3.GetObjectInput{...})
    if err != nil {
        // handle error
        return
    }
    // Make sure to always close the response Body when finished
    defer resp.Body.Close()

    decoder := json.NewDecoder(resp.Body)
    if err := decoder.Decode(&myStruct); err != nil {
        // handle error
        return
    }

.. _concurrently-using-service-clients:

Concurrently Using Service Clients
==================================

You can create goroutines that concurrently use the same service client
to send multiple requests. You can use a service client with as many
goroutines as you want. However, you cannot concurrently modify the
service client's configuration and request handlers. If you do, the
service client operations might encounter race conditions. Define
service client settings before you concurrently use it.

In the following example, an |S3| service client is used in multiple
goroutines. The example concurrently outputs all objects in ``bucket1``,
``bucket2``, and ``bucket3``, which are all in the same region. To make
sure all objects from the same bucket are printed together, the example
uses a channel.

.. code:: go

    sess, err := session.NewSession()
    if err != nil {
        fmt.Println("Error creating session ", err)
    }
    var wg sync.WaitGroup
    keysCh := make(chan string, 10)

    svc := s3.New(sess)
    buckets := []string{"bucket1", "bucket2", "bucket3"}
    for _, bucket := range buckets {
        params := &s3.ListObjectsInput{
            Bucket:  aws.String(bucket),
            MaxKeys: aws.Int64(100),
        }
        wg.Add(1)
        go func(param *s3.ListObjectsInput) {
            defer wg.Done()

            err = svc.ListObjectsPages(params,
                func(page *s3.ListObjectsOutput, last bool) bool {
                    // Add the objects to the channel for each page
                    for _, object := range page.Contents {
                        keysCh <- fmt.Sprintf("%s:%s", *params.Bucket, *object.Key)
                    }
                    return true
                },
            )
            if err != nil {
                fmt.Println("Error listing", *params.Bucket, "objects:", err)
            }
        }(params)
    }
    go func() {
        wg.Wait()
        close(keysCh)
    }()
    for key := range keysCh {
        // Print out each object key as its discovered
        fmt.Println(key)
    }

.. _using-pagination-methods:

Using Pagination Methods
========================

Typically, when you retrieve a list of items, you might need to check
the output for a token or marker to confirm whether AWS returned all
results from your request. If present, you use the token or marker to
request the next set of results. Instead of managing these tokens or
markers, you can use pagination methods provided by the SDK.

Pagination methods iterate over a list operation until the method
retrieves the last page of results or until the callback function
returns ``false``. The names of these methods use the following pattern:
*OperationName*\ ``Pages``. For example, the pagination method for the
|S3| list objects operation (``ListObjects``) is ``ListObjectPages``.

The following example uses the ``ListObjectPages`` pagination method to
list up to three pages of object keys from the ``ListObject``
operation. Each page consists of up to 10 keys, which is defined by
the ``MaxKeys`` field.

.. code:: go

    svc, err := s3.NewSession(sess)
    if err != nil {
        fmt.Println("Error creating session ", err)
    }
    inputparams := &s3.ListObjectsInput{
        Bucket:  aws.String("mybucket"),
        MaxKeys: aws.Int64(10),
    }
    pageNum := 0
    svc.ListObjectsPages(inputparams, func(page *s3.ListObjectsOutput, lastPage bool) bool {
        pageNum++
        for _, value := range page.Contents {
            fmt.Println(*value.Key)
        }
        return pageNum < 3
    })

.. _using-waiters:

Using Waiters
=============

The SDK provides waiters that continuously check for completion of a
job. For example, when you send a request to create an |S3| bucket, you
can use a waiter to check when the bucket has been successfully created.
That way, subsequent operations on the bucket are done only after the
bucket has been created.

The following example uses a waiter that waits until specific instances
have stopped.

.. code:: go

    sess, err := session.NewSession(aws.NewConfig().WithRegion("us-west-2"))
    if err != nil {
        fmt.Println("Error creating session ", err)
    }
    // Create an EC2 client
    ec2client := ec2.New(sess)
    // Specify two instances to stop
    instanceIDsToStop := aws.StringSlice([]string{"i-12345678", "i-23456789"})
    // Send request to stop instances
    _, err = ec2client.StopInstances(&ec2.StopInstancesInput{
      InstanceIds: instanceIDsToStop,
    })
    if err != nil {
      panic(err)
    }
    // Use a waiter function to wait until the instances are stopped
    describeInstancesInput := &ec2.DescribeInstancesInput{
      InstanceIds: instanceIDsToStop,
    }
    if err := ec2client.WaitUntilInstanceStopped(describeInstancesInput); err != nil {
      panic(err)
    }
    fmt.Println("Instances are stopped.")
