.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _s3-examples-bucket-ops:

#######################################
Performing Basic |S3| Bucket Operations
#######################################

.. meta::
   :description: Create and use Amazon S3 buckets using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, S3, list buckets, create bucket,
              list bucket items, add file to bucket, download file from
              bucket, copy bucket item to another bucket, delete bucket
              item, delete all bucket items, restore bucket item,
              delete bucket

These |sdk-go| examples show you how to perform the following operations on |S3|
buckets and bucket items:

* List the buckets in your account
* Create a bucket
* List the items in a bucket
* Upload a file to a bucket
* Download a bucket item
* Copy a bucket item to another bucket
* Delete a bucket item
* Delete all the items in a bucket
* Restore a bucket item
* Delete a bucket
* List the users with administrator privileges

You can download complete
versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <s3>` repository on GitHub.

.. _s3-examples-bucket-ops-scenario:

Scenario
========

In these examples, a series of Go routines are used to perform operations
on your |S3| buckets.
The routines use the |sdk-go| to perform |S3| bucket operations using the
following methods of the |S3| client class, unless otherwise noted:

* :sdk-go-api-deep:`ListBuckets <service/s3/#S3.ListBuckets>`
* :sdk-go-api-deep:`CreateBucket <service/s3/#S3.CreateBucket>`
* :sdk-go-api-deep:`ListObjects <service/s3/#S3.ListObjects>`
* :sdk-go-api-deep:`Upload <service/s3/s3manager/#Uploader.Upload>` (from the **s3manager.NewUploader** class)
* :sdk-go-api-deep:`Download <service/s3/s3manager/#Downloader.Download>` (from the **s3manager.NewDownloader** class)
* :sdk-go-api-deep:`CopyObject <service/s3/#S3.CopyObject>`
* :sdk-go-api-deep:`DeleteObject <service/s3/#S3.DeleteObject>`
* :sdk-go-api-deep:`DeleteObjects <service/s3/#S3.DeleteObjects>`
* :sdk-go-api-deep:`RestoreObject <service/s3/#S3.RestoreObject>`
* :sdk-go-api-deep:`DeleteBucket <service/s3/#S3.DeleteBucket>`

.. _s3-examples-bucket-ops-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with buckets. To learn more, see
  :S3-dg:`Working with Amazon S3 Buckets <UsingBucket>` in the |S3-dg|.

.. _s3-examples-bucket-ops-list-buckets:

List Buckets
============

The
:sdk-go-api-deep:`ListBuckets <service/s3/#S3.ListBuckets>`
function lists the buckets in your account.

The following example lists the buckets in your account.
There are no command line arguments.

Create the file *s3_list_buckets.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_list_buckets.go
   :lines: 17-23
   :language: go

Create a function we use to display errors and exit.

.. literalinclude:: example_code/s3/s3_list_buckets.go
   :lines: 48-51
   :language: go

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials,
and create a new |S3| service client.

.. literalinclude:: example_code/s3/s3_list_buckets.go
   :lines: 28-33
   :language: go
   :dedent: 4

Call :sdk-go-api-deep:`ListBuckets <service/s3/#S3.ListBuckets>`.
Passing ``nil`` means no filters are applied to the returned list.
If an error occurs, call ``exitErrorf``.
If no error occurs, loop through the buckets, printing the name and creation date of each bucket.

.. literalinclude:: example_code/s3/s3_list_buckets.go
   :lines: 35-45
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_list_buckets.go>`_
on GitHub.

.. _s3-examples-bucket-ops-create-bucket:

Create a Bucket
===============

The
:sdk-go-api-deep:`CreateBucket <service/s3/#S3.CreateBucket>`
function creates a bucket in your account.

The following example creates a bucket with the name specified as a
command line argument.
You must specify a globally unique name for the bucket.

Create the file *s3_create_bucket.go*.
Import the following Go and |sdk-go| packages.

.. literalinclude:: example_code/s3/s3_create_bucket.go
   :lines: 17-23
   :language: go

Create a function we use to display errors and exit.

.. literalinclude:: example_code/s3/s3_create_bucket.go
   :lines: 67-70
   :language: go

The program requires one argument, the name of the bucket to create.

.. literalinclude:: example_code/s3/s3_create_bucket.go
   :lines: 31-35
   :language: go
   :dedent: 4

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials,
and create a new S3 service client.

.. literalinclude:: example_code/s3/s3_create_bucket.go
   :lines: 39-44
   :language: go
   :dedent: 4

Call :sdk-go-api-deep:`CreateBucket <service/s3/#S3.CreateBucket>`,
passing in the bucket name defined previously.
If an error occurs, call ``exitErrorf``.
If there are no errors,
wait for a notification that the bucket was created.

.. literalinclude:: example_code/s3/s3_create_bucket.go
   :lines: 47-59
   :language: go
   :dedent: 4

If the ``WaitUntilBucketExists`` call returns an error,
call ``exitErrorf``.
If there are no errors, notify the user of success.

.. literalinclude:: example_code/s3/s3_create_bucket.go
   :lines: 60-64
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_create_bucket.go>`_
on GitHub.

.. _s3-examples-bucket-ops-list-bucket-items:

List Bucket Items
=================

The
:sdk-go-api-deep:`ListObjects <service/s3/#S3.ListObjects>`
function lists the items in a bucket.

The following example lists the items in the bucket with the name specified as a
command line argument.

Create the file *s3_list_objects.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_list_objects.go
   :lines: 17-23
   :language: go

Create a function we use to display errors and exit.

.. literalinclude:: example_code/s3/s3_list_objects.go
   :lines: 64-67
   :language: go

The program requires one command line argument, the name of the bucket.

.. literalinclude:: example_code/s3/s3_list_objects.go
   :lines: 30-35
   :language: go
   :dedent: 4

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials,
and create a new |S3| service client.

.. literalinclude:: example_code/s3/s3_list_objects.go
   :lines: 39-44
   :language: go
   :dedent: 4

Call :sdk-go-api-deep:`ListObjects <service/s3/#S3.ListObjects>`,
passing in the name of the bucket.
If an error occurs, call ``exitErrorf``.
If no error occurs, loop through the items, printing the name,
last modified date, size, and storage class of each item.

.. literalinclude:: example_code/s3/s3_list_objects.go
   :lines: 47-58
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_list_objects.go>`_
on GitHub.

.. _s3-examples-bucket-ops-upload-file-to-bucket:

Upload a File to a Bucket
=========================

The
:sdk-go-api-deep:`Upload <service/s3/s3manager/#Uploader.Upload>`
function uploads an object to a bucket.

The following example uploads a file to a bucket
with the names specified as command line arguments.

Create the file *s3_upload_object.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_upload_object.go
   :lines: 17-23
   :language: go

Create a function we use to display errors and exit.

.. literalinclude:: example_code/s3/s3_upload_object.go
   :lines: 82-85
   :language: go

Get the bucket and file name from the command line arguments,
open the file, and defer the file closing until we are done with it.
If an error occurs, call ``exitErrorF``.

.. literalinclude:: example_code/s3/s3_upload_object.go
   :lines: 31-44
   :language: go
   :dedent: 4

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials,
and create a ``NewUploader`` object.

.. literalinclude:: example_code/s3/s3_upload_object.go
   :lines: 48-56
   :language: go
   :dedent: 4

Upload the file to the bucket.
If an error occurs, call ``exitErrorF``.
Otherwise, notify the user that the upload succeeded.

.. literalinclude:: example_code/s3/s3_upload_object.go
   :lines: 60-61, 66, 72-79
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_upload_object.go>`_
on GitHub.

.. _s3-examples-bucket-ops-download-file-from-bucket:

Download a File from a Bucket
=============================

The
:sdk-go-api-deep:`Download <service/s3/s3manager/#Downloader.Download>`
function downloads an object from a bucket.

The following example downloads an item from a bucket
with the names specified as command line arguments.

Create the file *s3_download_object.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_download_object.go
   :lines: 17-25

Create a function we use to display errors and exit.

.. literalinclude:: example_code/s3/s3_download_object.go
   :lines: 68-71

Get the bucket and file name from the command line arguments.
If there aren't two arguments, call ``exitErrorf``.
Otherwise, create the file and defer file closing until we are done downloading.
If an error occurs while creating the file, call ``exitErrorf``.

.. literalinclude:: example_code/s3/s3_download_object.go
   :lines: 33-39
   :dedent: 4

Initialize the session in us-west-2 that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials,
and create a ``NewDownloader`` object.

.. literalinclude:: example_code/s3/s3_download_object.go
   :lines: 50-54
   :dedent: 4

Download the item from the bucket.
If an error occurs, call ``exitErrorf``.
Otherwise, notify the user that the download succeeded.

.. literalinclude:: example_code/s3/s3_download_object.go
   :lines: 56-65
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_download_object.go>`_
on GitHub.

.. _s3-examples-bucket-ops-copy-bucket-item:

Copy an Item from one Bucket to Another
=======================================

The
:sdk-go-api-deep:`CopyObject <service/s3/#S3.CopyObject>`
function copies an object from one bucket to another.

The following example copies an item from one bucket
to another with the names specified as command line arguments.

Create the file *s3_copy_object.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_copy_object.go
   :lines: 17-23

Create a function we use to display errors and exit.

.. literalinclude:: example_code/s3/s3_copy_object.go
   :lines: 64-67

Get the names of the bucket containing the item, the item to copy,
and the name of the bucket to which the item is copied.
If there aren't four command line arguments, call ``exitErrorf``.

.. literalinclude:: example_code/s3/s3_copy_object.go
   :lines: 30-38
   :dedent: 4

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials,
and create a new |S3| service client.

.. literalinclude:: example_code/s3/s3_copy_object.go
   :lines: 42-47
   :dedent: 4

Call :sdk-go-api-deep:`CopyObject <service/s3/#S3.CopyObject>`,
with the names of the bucket containing the item, the item to copy,
and the name of the bucket to which the item is copied.
If an error occurs, call ``exitErrorf``.
If no error occurs, wait for the item to be copied.

.. literalinclude:: example_code/s3/s3_copy_object.go
   :lines: 50-56
   :dedent: 4

If the ``WaitUntilObjectExists`` call returns an error,
call ``exitErrorf``. Otherwise, notify the user that the copy succeeded.

.. literalinclude:: example_code/s3/s3_copy_object.go
   :lines: 57-61
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_copy_object.go>`_
on GitHub.

.. _s3-examples-bucket-ops-delete-bucket-item:

Delete an Item in a Bucket
==========================

The
:sdk-go-api-deep:`DeleteObject <service/s3/#S3.DeleteObject>`
function deletes an object from a bucket.

The following example deletes an item from a bucket
with the names specified as command line arguments.

Create the file *s3_delete_object.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_delete_object.go
   :lines: 17-23

Create a function we use to display errors and exit.

.. literalinclude:: example_code/s3/s3_delete_object.go
   :lines: 65-68

Get the name of the bucket and object to delete.

.. literalinclude:: example_code/s3/s3_delete_object.go
   :lines: 31-37
   :dedent: 4

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials,
and create a new |S3| service client.

.. literalinclude:: example_code/s3/s3_delete_object.go
   :lines: 41-46
   :dedent: 4

Call :sdk-go-api-deep:`DeleteObject <service/s3/#S3.DeleteObject>`,
passing in the names of the bucket and object to delete.
If an error occurs, call ``exitErrorf``.
If no error occurs, wait until the object is deleted.

.. literalinclude:: example_code/s3/s3_delete_object.go
   :lines: 49-57
   :dedent: 4

If ``WaitUntilObjectNotExists`` returns an error, call ``exitErrorf``.
Otherwise, inform the user that the object was successfully deleted.

.. literalinclude:: example_code/s3/s3_delete_object.go
   :lines: 61-62
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_delete_object.go>`_
on GitHub.

.. _s3-examples-bucket-ops-delete-all-bucket-items:

Delete All the Items in a Bucket
================================

The
:sdk-go-api-deep:`DeleteObjects <service/s3/#S3.DeleteObjects>`
function deletes objects from a bucket.

The following example deletes all the items from a bucket
with the bucket name specified as a command line argument.

Create the file *s3_delete_objects.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_delete_objects.go
   :lines: 17-25

Create a function we use to display errors and exit.

.. literalinclude:: example_code/s3/s3_delete_objects.go
   :lines: 61-64

Get the name of the bucket.

.. literalinclude:: example_code/s3/s3_delete_objects.go
   :lines: 33-37
   :dedent: 4

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials,
and create a new |S3| service client.

.. literalinclude:: example_code/s3/s3_delete_objects.go
   :lines: 41-43, 46
   :dedent: 4

Create a list iterator to iterate through the list of bucket objects,
deleting each object.
If an error occurs, call ``exitErrorf``.

.. literalinclude:: example_code/s3/s3_delete_objects.go
   :lines: 49-52, 54-56
   :dedent: 4

Once all of the items in the bucket have been deleted,
inform the user that the objects were deleted.

.. literalinclude:: example_code/s3/s3_delete_objects.go
   :lines: 58
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_delete_objects.go>`_
on GitHub.

.. _s3-examples-bucket-ops-restore-bucket-item:

Restore a Bucket Item
=====================

The
:sdk-go-api-deep:`RestoreObject <service/s3/#S3.RestoreObject>`
function restores an item in a bucket.

The following example restores the items in a bucket
with the names specified as command line arguments.

Create the file *s3_restore_object.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_restore_object.go
   :lines: 17-23

Create a function we use to display errors and exit.

.. literalinclude:: example_code/s3/s3_restore_object.go
   :lines: 57-60

The program requires two arguments, the names of the bucket and object to
restore.

.. literalinclude:: example_code/s3/s3_restore_object.go
   :lines: 31-37
   :dedent: 4

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials,
and create a new |S3| service client.

.. literalinclude:: example_code/s3/s3_restore_object.go
   :lines: 41-46
   :dedent: 4

Call :sdk-go-api-deep:`RestoreObject <service/s3/#S3.RestoreObject>`,
passing in the bucket and object names and the number of days to
temporarily restore.
If an error occurs, call ``exitErrorf``.
Otherwise, inform the user that the bucket
should be restored in the next four hours or so.

.. literalinclude:: example_code/s3/s3_restore_object.go
   :lines: 49-54
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_restore_object.go>`_
on GitHub.

.. _s3-examples-bucket-ops-delete-bucket:

Delete a Bucket
===============

The
:sdk-go-api-deep:`DeleteBucket <service/s3/#S3.DeleteBucket>`
function deletes a bucket.

The following example deletes the bucket
with the name specified as a command line argument.

Create the file *s3_delete_bucket.go*.
Import the following Go and |sdk-go| packages.

.. literalinclude:: example_code/s3/s3_delete_bucket.go
   :lines: 17-23

Create a function we use to display errors and exit.

.. literalinclude:: example_code/s3/s3_delete_bucket.go
   :lines: 68-71

The program requires one argument, the name of the bucket to delete.
If the argument is not supplied, call ``exitErrorf``.

.. literalinclude:: example_code/s3/s3_delete_bucket.go
   :lines: 31-35
   :dedent: 4

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials,
and create a new S3 service client.

.. literalinclude:: example_code/s3/s3_delete_bucket.go
   :lines: 39-44
   :dedent: 4

Call :sdk-go-api-deep:`DeleteBucket <service/s3/#S3.DeleteBucket>`,
passing in the bucket name.
If an error occurs, call ``exitErrorf``.
If there are no errors,
wait for a notification that the bucket was deleted.

.. literalinclude:: example_code/s3/s3_delete_bucket.go
   :lines: 48-60
   :dedent: 4

If ``WaitUntilBucketNotExists`` returns an error, call ``exitErrorf``.
Otherwise, inform the user that the bucket was successfully deleted.

.. literalinclude:: example_code/s3/s3_delete_bucket.go
   :lines: 61-65
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_delete_bucket.go>`_
on GitHub.
