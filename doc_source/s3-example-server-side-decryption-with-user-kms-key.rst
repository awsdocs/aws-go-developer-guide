.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-s3-example-server-side-decryption-with-user-master-key:

######################################################################
Decrypting an Amazon S3 Bucket Object with a User-Supplied AWS KMS Key
######################################################################

.. meta::
    :description:
        Decrypt Amazon S3 bucket objects with a user-supplied AWS KMS key using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples

The following example uses the
:sdk-go-api-deep:`GetObject <service/s3/#S3.PutObject>`
method to get the object :code:`myObject` from the bucket
:code:`myBucket`.

Choose :code:`Copy` to save the code locally.

Create the file *decrypt_object.go*.

Import the required packages.

.. literalinclude:: ./example_code/s3/s3_decrypt_on_server_with_provided_kms_key.go
   :lines: 17-25
   :dedent: 0
   :language: go

Get the AWS KMS key from the command line,
where :code:`key` is an AWS KMS key ID as created in the :doc:`kms-example-create-key` example
and must be the same value you used to encrypt the object,
and set the name of the bucket and object.

.. literalinclude:: ./example_code/s3/s3_decrypt_on_server_with_provided_kms_key.go
   :lines: 28-35
   :dedent: 4
   :language: go

Create a session an |S3| encryption client.

.. literalinclude:: ./example_code/s3/s3_decrypt_on_server_with_provided_kms_key.go
   :lines: 40-42, 47
   :dedent: 4
   :language: go

Create input for and call :code:`get_object` to get the object.

.. literalinclude:: ./example_code/s3/s3_decrypt_on_server_with_provided_kms_key.go
   :lines: 49-54
   :dedent: 4
   :language: go

Save the object and display a success message.

.. literalinclude:: ./example_code/s3/s3_decrypt_on_server_with_provided_kms_key.go
   :lines: 62, 69, 71, 77
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_decrypt_on_server_with_provided_kms_key.go>`_
on GitHub.
