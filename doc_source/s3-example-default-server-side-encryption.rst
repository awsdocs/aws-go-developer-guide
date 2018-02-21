.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-s3-example-default-server-side-encryption:

##############################################################
Setting Default Server-Side Encryption for an Amazon S3 Bucket
##############################################################

.. meta::
    :description:
        Encrypt Amazon S3 bucket objects by default using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples

The following example uses the
:sdk-go-api-deep:`PutBucketEncryption <service/s3/#S3.PutBucketEncryption>`
method to enable KMS server-side encryption on any objects added to
:code:`myBucket`.

The only exception is if the user configures their request to explicitly
use server-side encryption. In that case, the specified encryption takes precedence.

Choose :code:`Copy` to save the code locally.

Create the file *set_default_encryption.go*.

Import the required packages.

.. literalinclude:: ./example_code/s3/s3_set_default_encryption.go
   :lines: 17-24
   :dedent: 0
   :language: go

Get the KMS key from the command line,
where :code:`key` is a KMS key ID as created in the :doc:`kms-example-create-key` example,
and set the bucket name.

.. literalinclude:: ./example_code/s3/s3_set_default_encryption.go
   :lines: 27-33
   :dedent: 4
   :language: go

Create a session and |S3| client.

.. literalinclude:: ./example_code/s3/s3_set_default_encryption.go
   :lines: 38-42
   :dedent: 4
   :language: go

Create the input for and call :code:`PutBucketEncryption`, and display a success message.

.. literalinclude:: ./example_code/s3/s3_set_default_encryption.go
   :lines: 45-49, 51, 58
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_set_default_encryption.go>`_
on GitHub.
