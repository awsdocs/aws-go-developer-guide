.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.


#########################
Examples of Using the SDK
#########################

.. meta::
   :description: Use code examples to write your own Go applications.
   :keywords: examples

The |sdk-go| examples can help you write your own applications.
The examples assume you have already set up and configured the SDK (that
is, all required packages are imported and your credentials and region
are set). For more information, see :doc:`setting-up` and :doc:`configuring-sdk`.


.. _examples-requests:

SDK Requests
============

Using context.Context with SDK Requests
---------------------------------------

With Go 1.7 the ``context.Context`` type was added to ``http.Request``. This type provides an easy way to implement deadlines, and cancelations on requests.

To use this pattern with the SDK all you need to do is call ``WithContext`` on the ``HTTPRequest`` field of the SDK's ``request.Request`` type, providing your Context value. The following example highlights this process with a timeout on a SQS ReceiveMessage API call.

.. literalinclude:: example_code/extending_sdk/request_context.go
   :lines: 15-


Using API field setters with SDK Requests
-----------------------------------------

In addition to setting API parameters via struct fields you can also use chainable setters on the API operation parameter fields. This allows you to use a chain of setters to set the fields of the API struct.

.. literalinclude:: example_code/extending_sdk/s3/put_object_with_setters.go
   :lines: 15-

This pattern can also be used with nested fields in API operation requets.

.. literalinclude:: example_code/extending_sdk/ecs/update_deployment_with_setters.go
   :lines: 15-

.. _examples-ec2:

|EC2|
=====

Create an Instance with Tags
----------------------------

The |EC2| service has an operation for creating instances
(:sdk-go-api-deep:`RunInstances <service/ec2/#EC2.RunInstances>`) and another for 
attaching tags to instances (:sdk-go-api-deep:`CreateTags <service/ec2/#EC2.CreateTags>`). 
To create an instance with tags, call both of these
operations in series. The following example creates an instance and then
adds a ``Name`` tag to the instance. The |EC2| console displays the
value of the ``Name`` tag in its list of instances.

.. literalinclude:: example_code/ec2/create_instance.go
   :lines: 15-


You can add up to 10 tags to an instance. You can add them all in a
single ``CreateTags`` operation.

Create Image Without Block Device
---------------------------------

Sometimes when you create an |EC2| image you may want to explicitly not
include certain block devices. To do this you can use the ``NoDevice``
parameter in :sdk-go-api-deep:`BlockDeviceMapping <service/ec2/#BlockDeviceMapping>`. 
When this parameter is set to an empty string ``""`` the named device will not be mapped. 
It is important to note that ``NoDevice`` parameter is not compatible with any other
field in ``BlockDeviceMapping`` other than ``DeviceName`` and the request
will fail if other parameters are present.

.. literalinclude:: example_code/ec2/create_image_no_block_device.go
   :dedent: 4
   :lines: 15-

.. _examples-s3:

|S3|
====

List All Buckets
----------------

The following example uses the |S3| 
:sdk-go-api-deep:`ListBuckets <service/s3/#S3.ListBuckets>` operation to
list all buckets in your AWS account:

.. literalinclude:: example_code/s3/list_all_buckets.go
   :dedent: 4
   :lines: 15-


Create a New Bucket and Object
------------------------------

The |S3| :sdk-go-api-deep:`CreateBucket <service/s3/#S3.CreateBucket>` operation 
creates a bucket in your account. You must specify a globally unique name for the bucket. 
The :sdk-go-api-deep:`PutObject <service/s3/#S3.PutObject>` operation creates a file in 
a bucket. The following example uses these two operations to create a bucket and add to it a
text file with the string ``Hello World!``:

.. literalinclude:: example_code/s3/create_new_bucket_and_object.go
   :dedent: 4
   :lines: 15-

Upload an Arbitrarily Sized Stream with |S3| Upload Manager
----------------------------------------------------------------

The |S3| :sdk-go-api-deep:`Upload Manager <service/s3/s3manager/#Uploader>` uploads 
large files to |S3| in smaller parts, in parallel. The following example gzips a large 
file and uploads it to Amazon as a stream:

.. literalinclude:: example_code/s3/upload_arbitrary_sized_stream.go
   :dedent: 4 
   :lines: 15-


Download a File with the |S3| Download Manager
---------------------------------------------------

The |S3|:sdk-go-api-deep:`Download Manager <service/s3/s3manager/#Downloader>` 
makes it easy to download large files in smaller parts, in parallel. The following example 
uses the Download Manager to write a file from |S3| to disk:

.. literalinclude:: example_code/s3/download_file.go
   :dedent: 4 
   :lines: 15-

Generate a Pre-signed URL for a GetObject Operation
---------------------------------------------------

A pre-signed URL allows you to grant temporary access to users who don't
have permission to directly run AWS operations in your account. A
pre-signed URL is signed with your credentials and can be used by any
user. To generate a pre-signed URL, use the 
:sdk-go-api-deep:`Presign() <aws/request/#Request.Presign>` method on the
``request`` object. You must set an expiration value because the SDK does
not set one by default.

The following example generates a pre-signed URL that enables you to
temporarily share a file without making it public. Anyone with access to
the URL can view the file.


.. literalinclude:: example_code/s3/generate_presigned_url.go
   :dedent: 4 
   :lines: 15-

If the SDK has not retrieved your credentials before calling
``Presign()``, it will get them to generate the pre-signed URL.

Generate a Pre-signed URL for an |S3| PUT Operation With a Specific Payload
---------------------------------------------------------------------------

You can generate a pre-signed URL for a PUT operation that checks
whether users upload the correct content. When the SDK pre-signs a
request, it computes the checksum of the request body and generates an
MD5 checksum that is included in the pre-signed URL. Users must upload
the same content that produces the same MD5 checksum generated by the
SDK; otherwise, the operation fails. This is not the Content-MD5, but
the signature. To enforce Content-MD5 simply add the header to the
request.

The following example adds a ``Body`` field to generate a pre-signed PUT
operation that requires a specific payload to be uploaded by users:

.. literalinclude:: example_code/s3/generate_presigned_url_specific_payload.go
   :dedent: 4 
   :lines: 15-


If you omit the ``Body`` field, users can write any contents to the given
object.

This example shows the enforcing of Content-MD5.

.. literalinclude:: example_code/s3/enforce_content_md5.go
   :dedent: 4 
   :lines: 15-


.. _examples-dynamodb:

|DDBlong|
=========

List Tables
-----------

The following example uses the |DDBlong| 
:sdk-go-api-deep:`ListTables <service/dynamodb/#DynamoDB.ListTables>` operation
to list all tables for the region you specified (``us-west-2``):


.. literalinclude:: example_code/dynamodb/list_tables.go
   :dedent: 4 
   :lines: 15-



.. _examples-glacier:

|GLlong|
========

Create a Vault
--------------

The following example uses the |GLlong| 
:sdk-go-api-deep:`CreateVault <service/glacier/#Glacier.CreateVault>` operation
to create a vault named ``YOUR_VAULT_NAME``:

.. literalinclude:: example_code/glacier/create_vault.go
   :dedent: 4 
   :lines: 15-


Upload an Archive
-----------------

The following example assumes you have a vault named
``YOUR_VAULT_NAME``. It uses the |GLlong| 
:sdk-go-api-deep:`UploadArchive <service/glacier/#Glacier.UploadArchive>`
operation to upload a single reader object as an entire archive. The SDK
automatically computes the tree hash checksum for the data to be
uploaded.

.. literalinclude:: example_code/glacier/upload_archive.go
   :dedent: 4 
   :lines: 15-

