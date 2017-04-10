.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-s3-creating-buckets:

########################################
Creating and Using |S3| Buckets with Go
########################################

.. meta::
   :description: Use S3 code examples to create and use S3 buckets in your own Go applications.
   :keywords: AWS SDK for Go examples, S3, create buckets

These Go examples show you how to:

* Obtain and display a list of |S3| buckets in your account
* Create an |S3| bucket
* Upload an object to a specified |S3| bucket

You can download complete versions of these example files from the :doc-examples-go:`aws-doc-sdk-examples <s3>` repository on GitHub.

.. _s3-create-scenario:

The Scenario
============

In this example, you use a series of Go routines to get a list of existing |S3| buckets, create a bucket,
and upload a file to a specified bucket. These routines use the |sdk-go| to get information from and upload files to an |S3| bucket by using these methods of the |S3| client class:

* :sdk-go-api-deep:`ListBuckets <service/s3/#S3.ListBuckets>`
* :sdk-go-api-deep:`CreateBucket <service/s3/#S3.CreateBucket>`
* :sdk-go-api-deep:`PutObject <service/s3/#S3.PutObject>`
* :sdk-go-api-deep:`CreateBucket <service/s3/#S3.CreateBucket>`

.. _s3-scenario-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with |S3| buckets. To learn more, see
  :S3-dg:`Working with Amazon S3 Buckets <UsingBucket>` in the |S3-dg|.

.. _s3-example-list-buckets:

Display a List of |S3| Buckets
==============================

Create a new Go file named :file:`s3_list_buckets.go`. You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/s3/s3_list_buckets.go
   :lines: 15-24

Initialize a session that the SDK will use to load configuration, credentials, and region information
from
the shared config file, ~/.aws/config. Then create a new S3 service client.

.. literalinclude:: example_code/s3/s3_list_buckets.go
   :lines: 26-34

Call :sdk-go-api-deep:`ListBuckets <service/s3/#S3.ListBuckets>`. Passing ``nil`` into the call means
no filters are applied to the returned list. If no error occurs, the code loops through the buckets, printing the name and creation date of the bucket.

.. literalinclude:: example_code/s3/s3_list_buckets.go
   :lines: 36-46

If an error does occur, print the error details and exit the routine.

.. literalinclude:: example_code/s3/s3_list_buckets.go
   :lines: 48-51

.. _s3-example-create-a-new-bucket:

Create a New |S3| Bucket and Object
===================================

The |S3| :sdk-go-api-deep:`CreateBucket <service/s3/#S3.CreateBucket>` operation creates a bucket in your
account. You must specify a globally unique name for the bucket. The :sdk-go-api-deep:`PutObject <service/s3/#S3.PutObject>`
operation creates a file in a bucket. The following example creates a bucket with a name you specify
in an argument.

Create a new Go file named :file:`s3_create_bucket.go`. You must import the relevant Go and
|sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/s3/s3_create_bucket.go
   :lines: 15-24

Create the |S3| Bucket
----------------------

This routine requires you to pass in an argument containing the name of the bucket you want to create.

.. literalinclude:: example_code/s3/s3_create_bucket.go
   :lines: 31-35

Initialize a session that the SDK will use to load configuration, credentials,
and region information from the shared config file, ~/.aws/config. Then create a new S3 service client.

.. literalinclude:: example_code/s3/s3_create_bucket.go
   :lines: 39-44

Call :sdk-go-api-deep:`CreateBucket <service/s3/#S3.CreateBucket>`, passing in the bucket name defined
previously. If there are no errors returned by the ``CreateBucket call``, wait for a notification that
the
bucket was created, and then notify the user of success.

.. literalinclude:: example_code/s3/s3_create_bucket.go
   :lines: 54-64

If an error does occur, print the error details and exit the routine.

.. literalinclude:: example_code/s3/s3_create_bucket.go
   :lines: 66-69


.. _s3-example-uploading-object-to-bucket:

Upload an Object to the |S3| Bucket
===================================

The :sdk-go-api-deep:`s3manager <service/s3/s3manager/>` provides utilities to upload and
download objects from |S3| concurrently. This is helpful when you're working with large objects.
The :sdk-go-api-deep:`PutObject <service/s3/#S3.PutObject>` operation creates a file in an S3
bucket. The following example creates a bucket with a name you specify in an argument.

Create a new Go file named :file:`s3_upload.go`. You must import the relevant Go and
|sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/s3/s3_upload.go
   :lines: 15-24

.. _s3-example-upload-file:

Upload a File to the |S3| Bucket
================================

This routine requires you to pass in the name of the file you want to upload and the name of the
bucket to upload it to.

.. literalinclude:: example_code/s3/s3_upload.go
   :lines: 31-37

As with the previous examples, you have to initialize a session that the SDK will use to load
configuration, credentials, and region information from the shared config file, ~/.aws/config. Then you
create a new S3 service client.

.. literalinclude:: example_code/s3/s3_upload.go
   :lines: 41-43

Try to open the file.

.. literalinclude:: example_code/s3/s3_upload.go
   :lines: 45-49

Now that you have the file, set up the S3 upload manager. See the
:sdk-go-api-deep:`Go SDK API documentation <service/s3/s3manager/#Uploader>`
for more information about configuring part size and concurrency.

Be aware that :file:`io.ReadSeeker` is preferred, as the Uploader can optimize memory when
uploading large content. Although io.Reader is supported, it requires buffering of the reader's
bytes for each part.

.. literalinclude:: example_code/s3/s3_upload.go
   :lines: 55-56, 59-60, 65, 71-

