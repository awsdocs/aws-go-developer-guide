.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-s3-bucket-policies:

#########################################
Working with |S3| Bucket Policies with Go
#########################################

.. meta::
   :description: Use this code example to learn how to work with Amazon S3 bucket policies.
   :keywords: AWS SDK for Go examples, S3, policy, ACL, permissions


This Go example show you how to retrieve, display, and set |S3| bucket polices. You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <s3>` repository on GitHub.

.. _s3-bucket-policies-scenario:

The Scenario
============

In this example, you use a series of Go routines to retrieve or set a bucket policy on an
|S3| bucket. The routines use the |sdk-go| to configure policy for a selected |S3| bucket by using
these methods of the |S3| client class:

* :sdk-go-api-deep:`GetBucketPolicy <service/s3/#S3.GetBucketPolicy>`
* :sdk-go-api-deep:`PutBucketPolicy <service/s3/#S3.PutBucketPolicy>`
* :sdk-go-api-deep:`DeleteBucketPolicy <service/s3/#S3.DeleteBucketPolicy>`

For more information about bucket policies for |S3| buckets, see
:s3-dg:`Using Bucket Policies and User Policies <using-iam-policies>` in the |S3-dg|.

.. _s3-bucket-policies-prerequisites:

Prerequisites
=============

* You have  :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with |S3| bucket polices. To learn more, see :s3-dg:`Using Bucket Policies and User Policies <using-iam-policies>` in the |S3-dg|.

.. _s3-example-get-policy:

Retrieve and Display a Bucket Policy
====================================

Create a new Go file named :file:`s3_get_bucket_policy.go`. You must import the relevant
Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/s3/s3_get_bucket_policy.go
   :lines: 15-28

This routine prints the policy for a bucket. If the bucket doesn't exist, or there was
an error, an error message is printed instead. It requires the bucket name as input.

.. literalinclude:: example_code/s3/s3_get_bucket_policy.go
   :lines: 35-40

Initialize a session that the SDK will use to load configuration, credentials,
and region information from the shared config file, ~/.aws/config. Then create a new S3 service client.

.. literalinclude:: example_code/s3/s3_get_bucket_policy.go
   :lines: 44-49

Call :sdk-go-api-deep:`GetBucketPolicy <service/s3/#S3.GetBucketPolicy>` to fetch the
policy, then display any errors.

.. literalinclude:: example_code/s3/s3_get_bucket_policy.go
   :lines: 52-67

Use Go's JSON package to print the Policy JSON returned by the call.

.. literalinclude:: example_code/s3/s3_get_bucket_policy.go
   :lines: 69-77

The ``exitError`` function is used to handle printing any errors.

.. literalinclude:: example_code/s3/s3_get_bucket_policy.go
   :lines: 79-82

.. _s3-example-set-policy:

Set Bucket Policy
=================

This routine sets the policy for a bucket. If the bucket doesn't exist, or there was
an error, an error message is printed instead. It requires the bucket name as input. It also requires
the same Go and |sdk-go| packages as the previous example, except for the ``bytes`` Go package.

.. literalinclude:: example_code/s3/s3_set_bucket_policy.go
   :lines: 17-27

Add the main function and parse the arguments to get the bucket name.

.. literalinclude:: example_code/s3/s3_set_bucket_policy.go
   :lines: 34-39

Initialize a session that the SDK will use to load configuration, credentials,
and region information from the shared config file, ~/.aws/config. Then create a new S3 service client.

.. literalinclude:: example_code/s3/s3_set_bucket_policy.go
   :lines: 43-48

Create a policy using the map interface, filling in the bucket as the resource.

.. literalinclude:: example_code/s3/s3_set_bucket_policy.go
   :lines: 52-67

Use Go's JSON package to marshal the policy into a JSON value so that it can be
sent to S3.

.. literalinclude:: example_code/s3/s3_set_bucket_policy.go
   :lines: 70-73

Call the S3 client's :sdk-go-api-deep:`PutBucketPolicy <service/s3/#S3.PutBucketPolicy>`
to PUT the policy for the bucket and print the results.

.. literalinclude:: example_code/s3/s3_set_bucket_policy.go
   :lines: 76-90

The ``exitError`` function is used to handle printing any errors.

.. literalinclude:: example_code/s3/s3_set_bucket_policy.go
   :lines: 92-95

