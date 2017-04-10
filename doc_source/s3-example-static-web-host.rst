.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-s3-website:

#################################################
Using an |S3| Bucket as a Static Web Host with Go
#################################################

.. meta::
   :description: Use S3 code examples to learn how to set up an Amazon S3 bucket as a static web host in your own Go applications.
   :keywords: AWS SDK for Go examples, S3 static web host


This Go example shows you how to configure an |S3| bucket to act as a static web host. You can download
complete versions of these example files from the :doc-examples-go:`aws-doc-sdk-examples <s3>` repository on GitHub.

.. _s3-website-scenario:

The Scenario
============

In this example, you use a series of Go routines to configure any of your
buckets to act as a static web host. The routines use the |sdk-go| to configure a selected |S3|
bucket using these methods of the |S3| client class:

* :sdk-go-api-deep:`GetBucketWebsite <service/s3/#S3.GetBucketWebsite>`
* :sdk-go-api-deep:`PutBucketWebsite <service/s3/#S3.PutBucketWebsite>`
* DeleteBucketWebsite

For more information about using an |S3| bucket as a static web host, see
:s3-dg:`Hosting a Static Website on Amazon S3 <WebsiteHosting>` in the |S3-dg|.

.. _s3-website-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with hosting static websites on |S3|. To learn more,
  see :s3-dg:`Hosting a Static Website on Amazon S3 <WebsiteHosting>` in the |S3-dg|.

.. _s3-example-retrieve-config:

Retrieve a Bucket's Website Configuration
=========================================

Create a new Go file named :file:`s3_get_bucket_website.go`. You must import the
relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/s3/s3_get_bucket_website.go
   :lines: 15-25

This routine requires you to pass in an argument containing the name of the bucket for which
you want to get website configuration.

.. literalinclude:: example_code/s3/s3_get_bucket_website.go
   :lines: 31-35

Initialize a session that the SDK will use to load configuration, credentials,
and region info from the shared config file, ~/.aws/config, and create a new S3 service client.

.. literalinclude:: example_code/s3/s3_get_bucket_website.go
   :lines: 39-44

Call :sdk-go-api-deep:`GetBucketWebsite <service/s3/#S3.GetBucketWebsite>` to get the bucket configuration.
You should check for the ``NoSuchWebsiteConfiguration`` error code, which tells you that the bucket doesn't
have a website configured.

.. literalinclude:: example_code/s3/s3_get_bucket_website.go
   :lines: 47-50, 53-57

Print the bucket's website configuration.

.. literalinclude:: example_code/s3/s3_get_bucket_website.go
   :lines: 60-

.. _s3-example-set-bucket-website:

Set a Bucket's Website Configuration
====================================

Create a Go file named :file:`s3_set_bucket_website.go` and add the code below. The |S3|
:sdk-go-api-deep:`PutBucketWebsite <service/s3/#S3.PutBucketWebsite>` operation
sets the website configuration on a bucket, replacing any existing configuration.

Create a new Go file named :file:`s3_create_bucket.go`. You must import the relevant Go and
|sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/s3/s3_set_bucket_website.go
   :lines: 15-25

This routine requires you to pass in an argument containing the name of the bucket and the index suffix page.

.. literalinclude:: example_code/s3/s3_set_bucket_website.go
   :lines: 35-42

Initialize a session that the SDK will use to load configuration, credentials,
and region information from the shared config file, ~/.aws/config, and create a new S3 service client.

.. literalinclude:: example_code/s3/s3_set_bucket_website.go
   :lines: 46-51

Create the parameters to be passed in to :sdk-go-api-deep:`PutBucketWebsite <service/s3/#S3.PutBucketWebsite>`,
including the bucket name and index document details. If the user passed in an error page
when calling the routine, also add that to the parameters.

.. literalinclude:: example_code/s3/s3_set_bucket_website.go
   :lines: 54-62, 64-68

Call ``PutBucketWebsite``, passing in the parameters you just defined. If an error occurs,
print the errordetails and exit the routine.

.. literalinclude:: example_code/s3/s3_set_bucket_website.go
   :lines: 72-79


.. _s3-example-delete-website:

Delete Website Configuration on a Bucket
========================================

Create a Go file named :file:`s3_delete_bucket_website.go`. The following code
shows you how to delete the bucket's
website configuration.

Create a new Go file named :file:`s3_upload.go`. You must import the relevant Go and
|sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/s3/s3_delete_bucket_website.go
   :lines: 15-25

This routine requires you to pass in the name of the bucket for which you want to delete the
website configuration.

.. literalinclude:: example_code/s3/s3_delete_bucket_website.go
   :lines: 36-41

Initialize a session that the SDK will use to load
configuration, credentials, and region information from the shared config file, ~/.aws/config, and
create a new S3 service client.

.. literalinclude:: example_code/s3/s3_delete_bucket_website.go
   :lines: 45-50

Call `DeleteBucketWebsite` and pass in the name of the bucket to complete the action.

.. literalinclude:: example_code/s3/s3_delete_bucket_website.go
   :lines: 55-

