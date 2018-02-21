.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-glacier:

####################################
|GLlong| Examples Using the |sdk-go|
####################################

.. meta::
   :description: Use Amazon Glacier code examples to write your own Go applications.
   :keywords: AWS SDK for Go code examples, Amazon Glacier

|GLlong| is a secure, durable, and extremely low-cost cloud storage service for data archiving and long-term
backup. The |sdk-go| examples can integrate |GLlong| into your applications. The examples assume you have already set up and configured the SDK (that is, you've imported all required packages and set your credentials and region). For more information, see :doc:`setting-up` and :doc:`configuring-sdk`.

You can download complete versions of these example files from the :doc-examples-go:`aws-doc-sdk-examples <glacier>` repository on GitHub.

The Scenario
============

|GLlong| is a secure cloud storage service for data archiving and long-term backup. The service is optimized for infrequently accessed data where a retrieval time of several hours is suitable. These examples show you how to create a vault and upload an archive with Go. The methods used include:

* :sdk-go-api-deep:`CreateVault <service/glacier/#Glacier.CreateVault>`
* :sdk-go-api-deep:`UploadArchive <service/glacier/#Glacier.UploadArchive>`

.. _create-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with the |GLlong| data model. To learn more, see
  :GL-dg:`Amazon Glacier Data Model <amazon-glacier-data-model>` in the |GL-dg|.

.. _create-a-vault:

Create a Vault
==============

The following example uses the |GLlong| :sdk-go-api-deep:`CreateVault <service/glacier/#Glacier.CreateVault>`
operation to create a vault named ``YOUR_VAULT_NAME``.

.. literalinclude:: example_code/glacier/create_vault.go
   :dedent: 4
   :lines: 15-


Upload an Archive
=================

The following example assumes you have a vault named ``YOUR_VAULT_NAME``. It uses the |GLlong|
:sdk-go-api-deep:`UploadArchive <service/glacier/#Glacier.UploadArchive>` operation to upload a
single reader object as an entire archive. The |sdk-go| automatically computes the tree hash
checksum for the data to be uploaded.

.. literalinclude:: example_code/glacier/upload_archive.go
   :dedent: 4
   :lines: 15-
