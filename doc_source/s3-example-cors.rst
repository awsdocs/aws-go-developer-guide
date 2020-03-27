.. Copyright 2010-2020 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-s3-cors:

##################################
Working with |S3| CORS Permissions
##################################

.. meta::
   :description: Learn to work with cross-origin resource sharing (CORS) for an Amazon S3 bucket using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, S3, cross origin resource sharing, CORS


This |sdk-go| example shows you how to list |S3| buckets and configure
CORS and bucket logging. You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <s3>` repository on GitHub.

.. _s3-cors-scenario:

Scenario
========

In this example, a series of Go routines are used to list your |S3| buckets and to configure
CORS and bucket logging. The routines use the |sdk-go| to configure a selected
|S3| bucket using these methods of the |S3| client class:

* :sdk-go-api-deep:`GetBucketCors <service/s3/#S3.GetBucketCors>`
* :sdk-go-api-deep:`PutBucketCors <service/s3/#S3.PutBucketCors>`

If you are unfamiliar with using CORS configuration with an |S3| bucket, it's worth your time
to read :s3-dg:`Cross-Origin Resource Sharing (CORS) <cors>` in the |S3-dg| before proceeding.

.. _s3-cors-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with using CORS configuration with an |S3| bucket. To learn more, see
  :s3-dg:`Cross-Origin Resource Sharing (CORS) <cors>` in the |S3-dg|.

.. _s3-example-cors-config:

Configure CORS on the Bucket
============================

Create a new Go file named :file:`s3_set_cors.go`. You must import the relevant Go and
|sdk-go| packages by adding the following lines.

.. literalinclude:: s3.go.set_cors.imports.txt
   :dedent: 0
   :language: go

This routine configures CORS rules for a bucket by setting the allowed HTTP methods.

It requires the bucket name and can also take a space-separated list of HTTP methods. Using the
Go Language's ``flag`` package, it parses the input and validates the bucket name.

.. literalinclude:: s3.go.set_cors.vars.txt
   :dedent: 4
   :language: go

Notice the helper method, ``filterMethods``, which ensures the methods passed in are uppercase.

.. literalinclude:: s3.go.set_cors.filter.txt
   :dedent: 0
   :language: go

Initialize a session that the SDK will use to load credentials,
from the shared credentials file, ~/.aws/credentials, and create a new S3 service client.

.. literalinclude:: s3.go.set_cors.session.txt
   :dedent: 4
   :language: go

Create a new :sdk-go-api-deep:`CORSRule <service/s3/#CORSRule>` to set up the CORS configuration.

.. literalinclude:: s3.go.set_cors.rule.txt
   :dedent: 4
   :language: go

Add the ``CORSRule`` to the ``PutBucketCorsInput`` structure, call ``PutBucketCors`` with
that structure, and print a success or error message.

.. literalinclude:: s3.go.set_cors.put.txt
   :dedent: 4
   :language: go

The example uses the following error function.

.. literalinclude:: s3.go.set_cors.exit.txt
   :dedent: 0
   :language: go
