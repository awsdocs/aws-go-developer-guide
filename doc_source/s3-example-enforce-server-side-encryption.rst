.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-s3-example-enforce-server-side-encryption:

#####################################################################
Requiring Encryption on the Server to Upload Amazon S3 Bucket Objects
#####################################################################

.. meta::
    :description:
        Require server-side encryption to upload Amazon S3 bucket using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples

The following example uses the
:sdk-go-api-deep:`PutBucketPolicy <service/s3/#S3.PutBucketPolicy>`
method to require that objects uploaded to an |S3| bucket have |S3| encrypt the object with an AWS KMS key.
Attempts to upload an object without specifying that |S3| encrypt the object with an AWS KMS key
raise an :code:`Aws::S3::Errors::AccessDenied` exception.

Avoid using this configuration option if you use default server-side encryption as described in
:doc:`s3-example-default-server-side-encryption` as they could conflict and result in unexpected
results.

Choose :code:`Copy` to save the code locally.

Create the file *require_server_encryption.go*.

Import the required packages.

.. literalinclude:: ./example_code/s3/s3_require_server_encryption.go
   :lines: 17-26
   :dedent: 0
   :language: go

Set the name of the bucket, create a session, and create an Amazon S3 client.

.. literalinclude:: ./example_code/s3/s3_require_server_encryption.go
   :lines: 29,34-38
   :dedent: 4
   :language: go

Create an |S3| policy that requires server-side KMS encryption on objects
uploaded to the bucket.

.. literalinclude:: ./example_code/s3/s3_require_server_encryption.go
   :lines: 40-68
   :dedent: 4
   :language: go

Convert the policy into JSON, create the input for and call :code:`PutBucketPolicy`, apply the policy to the bucket,
and print a success message.

.. literalinclude:: ./example_code/s3/s3_require_server_encryption.go
   :lines: 71, 78-81, 83, 90
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_require_server_encryption.go>`_
on GitHub.
