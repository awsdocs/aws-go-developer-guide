.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-s3-example-server-side-encryption-with-kms:

#################################################################
Encrypting an Amazon S3 Bucket Object on the Server Using AWS KMS
#################################################################

.. meta::
    :description:
        Encrypt Amazon S3 bucket objects on the server with AWS KMS using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples

The following example uses the
:sdk-go-api-deep:`PutObject <service/s3/#S3.PutObject>`
method to add the object :code:`myItem` to the bucket
:code:`myBucket` with server-side encryption set to AWS KMS.

Note that this differs from :doc:`s3-example-default-server-side-encryption`,
is in that case, the objects are encrypted without you having to explicitly perform the operation.

Choose :code:`Copy` to save the code locally.

Create the file *encrypt_object_on_server.go*.

Add the required packages.

.. literalinclude:: ./example_code/s3/s3_encrypt_on_server_with_kms.go
   :lines: 17-25
   :dedent: 0
   :language: go

Get the KMS key from the command line,
where :code:`key` is a KMS key ID as created in the :doc:`kms-example-create-key` example,
and set the bucket and object names.

.. literalinclude:: ./example_code/s3/s3_encrypt_on_server_with_kms.go
   :lines: 28-35
   :dedent: 4
   :language: go

Create a session and |S3| client.

.. literalinclude:: ./example_code/s3/s3_encrypt_on_server_with_kms.go
   :lines: 40-44
   :dedent: 4
   :language: go

Create input for and call :code:`put_object`.
Notice that the :code:`server_side_encryption` property is set to :code:`aws:kms`,
indicating that |S3| encrypts the object using AWS KMS,
and display a success message to the user.

.. literalinclude:: ./example_code/s3/s3_encrypt_on_server_with_kms.go
   :lines: 46-54, 61
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_encrypt_on_server_with_kms.go>`_
on GitHub.
