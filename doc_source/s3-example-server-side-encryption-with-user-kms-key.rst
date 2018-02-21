.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-s3-example-server-side-encryption-with-user-kms-key:

######################################################################
Encrypting an Amazon S3 Bucket Object with a User-Supplied AWS KMS Key
######################################################################

.. meta::
    :description:
        Encrypt Amazon S3 bucket objects with a user-supplied AWS KMS key using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples

The following example uses the
:sdk-go-api-deep:`PutObject <service/s3/#S3.PutObject>`
method to add the object :code:`myObject` to the bucket 
:code:`myBucket`.

Choose :code:`Copy` to save the code locally.

Create the file *encrypt_object.go*.

Include the required packages.

.. literalinclude:: ./example_code/s3/s3_encrypt_on_server_with_provided_kms_key.go
   :lines: 17-27
   :dedent: 0
   :language: go

Get AWS KMS key from the command line,
where :code:`key` is an AWS KMS key ID as created in the :doc:`kms-example-create-key` example,
and set the bucket and object names.

.. literalinclude:: ./example_code/s3/s3_encrypt_on_server_with_provided_kms_key.go
   :lines: 30-37
   :dedent: 4
   :language: go

Create a session and encryption client.

.. literalinclude:: ./example_code/s3/s3_encrypt_on_server_with_provided_kms_key.go
   :lines: 42-46, 50
   :dedent: 4
   :language: go

Create the input for and call :code:`PutObject` to upload the object to the bucket,
and display a success message.

.. literalinclude:: ./example_code/s3/s3_encrypt_on_server_with_provided_kms_key.go
   :lines: 52-58, 65
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_encrypt_on_server_with_provided_kms_key.go>`_
on GitHub.
