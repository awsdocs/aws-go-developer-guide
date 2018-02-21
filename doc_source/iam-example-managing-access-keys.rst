.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-iam-access-keys:

##########################
Managing |IAM| Access Keys
##########################

.. meta::
   :description: Manage IAM access keys using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, IAM access keys


This Go example shows you how to create, modify, view, or rotate |IAM| access keys. You can download complete versions of these example files from the :doc-examples-go:`aws-doc-sdk-examples <s3>` repository on GitHub.

.. _iam-access-keys-scenario:

Scenario
========

Users need their own access keys to make programmatic calls to the |sdk-go|. To fill this need, you can create, modify, view, or rotate access keys (access key IDs and secret access keys) for |IAM| users. By default, when you create an access key its status is Active, which means the user can use the access key for API calls.

In this example, you use a series of Go routines to manage access keys in |IAM|. The routines use the |sdk-go| IAM client methods that follow:

* :sdk-go-api-deep:`CreateAccessKey <service/iam/#IAM.CreateAccessKey>`
* :sdk-go-api-deep:`ListAccessKeys <service/iam/#IAM.ListAccessKeys>`
* :sdk-go-api-deep:`GetAccessKeyLastUsed <service/iam/#IAM.GetAccessKeyLastUsed>`
* :sdk-go-api-deep:`UpdateAccessKey <service/iam/#IAM.UpdateAccessKey>`
* :sdk-go-api-deep:`DeleteAccessKey <service/iam/#IAM.DeleteAccessKey>`

.. _iam-access-keys-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with |IAM| access keys. To learn more, see :iam-ug:`Managing Access Keys for IAM Users <id_credentials_access-keys>` in the |IAM-ug|.

.. _iam-example-create-access-key:

Create a New IAM Access Key
===========================

This code creates a new |IAM| access key for the |IAM| user named IAM_USER_NAME.

Create a new Go file named :file:`iam_createaccesskey.go`. You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_createaccesskey.go
   :lines: 15-23

Set up the session.

.. literalinclude:: example_code/iam/iam_createaccesskey.go
   :lines: 27,30-35

Call ``CreateAccessKey`` and print the results.

.. literalinclude:: example_code/iam/iam_createaccesskey.go
   :lines: 37-47

.. _iam-example-list-access-keys:

List a User's Access Keys
=========================

In this example, you get a list of the access keys for a user and print the list to the console.

Create a new Go file named :file:`iam_listaccesskeys.go`. You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_listaccesskeys.go
   :lines: 15-23

Set up a new |IAM| client.

.. literalinclude:: example_code/iam/iam_listaccesskeys.go
   :lines: 27,30-35

Call ``ListAccessKeys`` and print the results.

.. literalinclude:: example_code/iam/iam_listaccesskeys.go
   :lines: 37-48

.. _iam_example_last_use:

Get the Last Use for an Access Key
==================================

In this example, you find out when an access key was last used.

Create a new Go file named :file:`iam_accesskeylastused.go`. You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_accesskeylastused.go
   :lines: 15-23

Set up a new |IAM| client.

.. literalinclude:: example_code/iam/iam_accesskeylastused.go
   :lines: 27,30-35

Call ``GetAccessKeyLastUsed``, passing in the access key ID, and print the results.

.. literalinclude:: example_code/iam/iam_accesskeylastused.go
   :lines: 37-48

.. _iam_example_update-access-key:

Update Access Key Status
========================

.. There's nothing in the code that's about deleting a user--is this correct?

In this example, you delete an |IAM| user.

Create a new Go file with the name :file:`iam_updateaccesskey.go`. You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_updateaccesskey.go
   :lines: 15-23

Set up a new |IAM| client.

.. literalinclude:: example_code/iam/iam_updateaccesskey.go
   :lines: 27,30-35

Call ``UpdateAccessKey``, passing in the access key ID, status (making it active in this case), and user name.

.. literalinclude:: example_code/iam/iam_updateaccesskey.go
   :lines: 37-49

.. _iam-example-delete-access-key:

Delete an Access Key
====================

In this example, you delete an access key.

Create a new Go file named :file:`iam_deleteaccesskey.go`. You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_deleteaccesskey.go
   :lines: 15-23

Set up a new |IAM| client.

.. literalinclude:: example_code/iam/iam_deleteaccesskey.go
   :lines: 27,30-35

Call ``DeleteAccessKey``, passing in the access key ID and user name.

.. literalinclude:: example_code/iam/iam_deleteaccesskey.go
   :lines: 37-48
