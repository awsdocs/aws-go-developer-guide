.. Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#######################################
S3 Encryption Client Migration |sdk-go|
#######################################

.. meta::
   :description: Describes how to migrate to the latest S3 encryption clients for |sdk-go}.

AWS has released S3 encryption clients that offer improved security. Previously available clients
are being deprecated. You should upgrade your SDK version, and modify your code to use the new
clients as soon as possible.

Migration Overview
==================

The following sections show how to migrate applications to the S3 V2 encryption clients, so you can ensure
availability of your application throughout the migration. This migration will happen in two phases:
First, you must deploy an updated version of the AWS SDK for Go to your application fleet. This
updated client will allow you to decrypt objects created using the V2
encryption client. If your application uses multiple AWS SDKs, you'll need to upgrade each SDK to
a compatible revision as well. Once your existing clients have been updated, you can migrate your
existing encryption and decryption clients to their respective V2 versions.

Updating Existing Clients to Read New Formats
=============================================

The first step in the migration will be to update your V1 decryption clients to the latest SDK release.
After completing this step, your application V1 clients will be able to decrypt objects encrypted by
V2 encryption clients.

Applications using Go Modules
-----------------------------

Projects that use the Go module dependency system introduced in Go 1.11 can execute the following
commands to download the latest release of the AWS SDK for Go and verify that they have successfully
updated to a newer release.

1. Update your SDK module dependency.

::

  $ go get -u github.com/aws/aws-sdk-go

2. Validate your dependency has correctly updated to the required minimum version.

::

  $ go list -m github.com/aws/aws-sdk-go
  github.com/aws/aws-sdk-go v1.x.z

After you upgrade your dependencies and verify, deploy the application to your fleet.
Once the rollout is complete, you can migrate your V1 encryption and decryption clients
to V2.

Applications using GOPATH
-------------------------

If your application uses GOPATH to manage its dependencies the following steps will need
to be taken to update and verify that your application is using the minimum SDK release or
newer.

1. Update your SDK GOPATH source code.

::

  $ go get -u github.com/aws/aws-sdk-go

2. Ensure the SDK source path to the minimum tagged release (v1.y.z) or newer `release <https://github.com/aws/aws-sdk-go/releases>`_.

::

  $ cd $GOPATH/src/github.com/aws/aws-sdk-go
  $ git fetch
  $ go checkout v1.y.z

After you upgrade your dependencies and verify, deploy the application to your fleet.
Once the rollout is complete, you can migrate your V1 encryption and decryption clients
to V2.

Migrate Encryption and Decryption Clients to V2
===============================================

After updating your existing clients to read the new encryption formats, you can now proceed
with safely updating your applications to the V2 encryption and decryption clients. The next
series of steps will guide you through what actions must be taken to successfully migrate
your code from V1 to V2.

Migrate Cipher Data Generators
------------------------------

Applications that use `NewKMSKeyGenerator <https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#NewKMSKeyGenerator>`_
or `NewKMSKeyGeneratorWithMatDesc <https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#NewKMSKeyGeneratorWithMatDesc>`_
functions for constructing a `CipherDataGenerator <https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#CipherDataGenerator>`_
will need to migrate their usage to either `NewKMSContextKeyGenerator <https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#NewKMSContextKeyGenerator>`_
or `NewKMSContextKeyGeneratorWithMatDesc <https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#NewKMSContextKeyGeneratorWithMatDesc>`_
respectively. This migration is required, as support for the former CipherDataGenerators have
been deprecated.  Attempting to construct a V2 client using the old methods will result
in an  error during client construction.

**Pre-migration Sample**

.. code:: go

  encryptionClient := s3crypto.NewEncryptionClient(sess, contentCipherBuilder)

**Post-migration Sample**

.. code:: go

  encryptionClient, err := s3crypto.NewEncryptionClientV2(sess, contentCipherBuilder)
  if err != nil {
    panic(err)
  }

Migrate Custom Encryption Client Configurations
-----------------------------------------------

Clients that utilize custom client configuration options will be required to update
their function argument signatures to use `EncryptionClientOptions <https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#EncryptionClientOptions>`_
for setting custom options such as an alternative `SaveStrategy <https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#SaveStrategy>`_.

**Pre-migration Sample**

.. code:: go

  // example setting an alternative S3 client
  encryptionClient := s3crypto.NewEncryptionClient(cipherDataGenerator, contentCipherBuilder, func(o *s3crypto.EncryptionClient) {
    o.S3Client = s3.New(sess, &aws.Config{Region: aws.String("us-west-2")})
  })

**Post-migration Sample**

.. code:: go

  // example setting an alternative S3 client
  encryptionClient, err := s3crypto.NewEncryptionClientV2(sess, contentCipherBuilder, func(o *s3crypto.EncryptionClientOptions) {
    o.S3Client = s3.New(sess, &aws.Config{Region: aws.String("us-west-2")})
  })
  if err != nil {
    panic(err)
  }

Migrate Decryption Client Constructors
--------------------------------------

The V2 client’s default constructor pattern and return signature do not differ
when using the default client constructor options.

**Pre-migration Sample**

.. code:: go

  decryptionClient := s3crypto.NewDecryptionClient(sess)

**Post-migration Sample**

.. code:: go

  decryptionClient := s3crypto.NewDecryptionClientV2(sess)

Migrating Custom Decryption Client Configurations
-------------------------------------------------

Clients that utilize custom client configurations options will be required to update
their functional argument signatures to use `DecryptionClientOptions <https://docs.aws.amazon.com/sdk-for-go/api/service/s3/s3crypto/#DecryptionClientOptions>`_.

**Pre-migration Sample**

.. code:: go

  decryptionClient := s3crypto.NewDecryptionClient(sess, func(o *s3crypto.DecryptionClient) {
    o.S3Client = s3.New(sess, &aws.Config{Region: aws.String("us-west-2")})
  })

**Post-migration Sample**

.. code:: go

  decryptionClient := s3crypto.NewDecryptionClientV2(sess, func(o *s3crypto.DecryptionClientOptions) {
    o.S3Client = s3.New(sess, &aws.Config{Region: aws.String("us-west-2")})
  })

After complete the steps migrating your code from the V1 to V2 clients, your may proceed
to testing and deployment using your applications best practices. After completing
your application deployment you will have successfully migrated your application
from the V1 to V2 clients.
