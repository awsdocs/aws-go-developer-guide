.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-s3-bucket-policies:

#################################
Working with |S3| Bucket Policies
#################################

.. meta::
   :description: Work with Amazon S3 bucket policies using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go examples, S3, policy, ACL, permissions


This |sdk-go| example shows you how to retrieve, display, and set |S3| bucket polices. You can download
complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <s3>` repository on GitHub.

.. _s3-bucket-policies-scenario:

Scenario
========

In this example, you use a series of Go routines to retrieve or set a bucket policy on an
|S3| bucket. The routines use the |sdk-go| to configure policy for a selected |S3| bucket using
these methods of the |S3| client class:

* :sdk-go-api-deep:`GetBucketPolicy <service/s3/#S3.GetBucketPolicy>`
* :sdk-go-api-deep:`PutBucketPolicy <service/s3/#S3.PutBucketPolicy>`
* :sdk-go-api-deep:`DeleteBucketPolicy <service/s3/#S3.DeleteBucketPolicy>`

For more information about bucket policies for |S3| buckets, see
:s3-dg:`Using Bucket Policies and User Policies <using-iam-policies>` in the |S3-dg|.

.. _s3-bucket-policies-prerequisites:

Prerequisites
=============

* You have  :doc:`set up <setting-up>`, and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with |S3| bucket polices. To learn more, see :s3-dg:`Using Bucket Policies and User Policies <using-iam-policies>` in the |S3-dg|.

.. _s3-example-get-policy:

Retrieve and Display a Bucket Policy
====================================

Create a new Go file named :file:`s3_get_bucket_policy.go`. You must import the relevant
Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/s3/s3_get_bucket_policy.go
   :lines: 17-27
   :language: go

Create the ``exitError`` function to deal with errors.

.. literalinclude:: example_code/s3/s3_get_bucket_policy.go
   :lines: 79-82
   :language: go
       
This routine prints the policy for a bucket. If the bucket doesn't exist, or there was
an error, an error message is printed instead. It requires the bucket name as input.

.. literalinclude:: example_code/s3/s3_get_bucket_policy.go
   :lines: 35-40
   :language: go
   :dedent: 4

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials, and create a new S3 service client.

.. literalinclude:: example_code/s3/s3_get_bucket_policy.go
   :lines: 44-47, 49
   :language: go
   :dedent: 4

Call :sdk-go-api-deep:`GetBucketPolicy <service/s3/#S3.GetBucketPolicy>` to fetch the
policy, then display any errors.

.. literalinclude:: example_code/s3/s3_get_bucket_policy.go
   :lines: 52-67
   :language: go
   :dedent: 4

Use Go's JSON package to print the Policy JSON returned by the call.

.. literalinclude:: example_code/s3/s3_get_bucket_policy.go
   :lines: 69-76
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_get_bucket_policy.go>`_
on GitHub.
            
.. _s3-example-set-policy:

Set Bucket Policy
=================

This routine sets the policy for a bucket. If the bucket doesn't exist, or there was
an error, an error message will be printed instead. It requires the bucket name as input. It also requires the same Go and |sdk-go| packages as the previous example, except for the ``bytes`` Go package.

.. literalinclude:: example_code/s3/s3_set_bucket_policy.go
   :lines: 17-26
   :language: go

Add the main function and parse the arguments to get the bucket name.

.. literalinclude:: example_code/s3/s3_set_bucket_policy.go
   :lines: 33-38
   :language: go

Initialize a session that the SDK will use to load configuration, credentials,
and region information from the shared credentials file, ~/.aws/credentials, and create a new S3 service client.

.. literalinclude:: example_code/s3/s3_set_bucket_policy.go
   :lines: 42-45, 47
   :language: go

Create a policy using the map interface, filling in the bucket as the resource.

.. literalinclude:: example_code/s3/s3_set_bucket_policy.go
   :lines: 51-66
   :language: go

Use Go's JSON package to marshal the policy into a JSON value so that it can be
sent to S3.

.. literalinclude:: example_code/s3/s3_set_bucket_policy.go
   :lines: 69
   :language: go

Call the S3 client's :sdk-go-api-deep:`PutBucketPolicy <service/s3/#S3.PutBucketPolicy>`
to PUT the policy for the bucket and print the results.

.. literalinclude:: example_code/s3/s3_set_bucket_policy.go
   :lines: 75-78, 87-88
   :language: go

The ``exitError`` function is used to deal with printing any errors.

.. literalinclude:: example_code/s3/s3_set_bucket_policy.go
   :lines: 91-94
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_set_bucket_policy.go>`_
on GitHub.
