.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-iam-users:

######################################
Managing |IAM| Users with the |sdk-go|
######################################

.. meta::
   :description: Use this code example to learn how to manage IAM users in Go.
   :keywords: AWS SDK for Go examples, IAM users


This Go example shows you how to create, update, view, and
delete |IAM| users. You can download complete versions of these
example files from the :doc-examples-go:`aws-doc-sdk-examples <s3>` repository on GitHub.

.. _iam-users-scenario:

The Scenario
============

In this example, you use a series of Go routines to manage users in |IAM|.
The routines use the |sdk-go| IAM client methods that follow:

* :sdk-go-api-deep:`CreateUser <service/iam/#IAM.CreateUser>`
* :sdk-go-api-deep:`ListUsers <service/iam/#IAM.ListUsers>`
* :sdk-go-api-deep:`UpdateUser <service/iam/#IAM.UpdateUser>`
* :sdk-go-api-deep:`GetUser <service/iam/#IAM.GetUser>`
* :sdk-go-api-deep:`DeleteUser <service/iam/#IAM.DeleteUser>`

.. _iam-users-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with |IAM| users. To learn more, see :iam-ug:`IAM Users <id_users>` in the |IAM-ug|.

.. _iam-example-create-user:

Create a New |IAM| User
=======================

This code creates a new |IAM| user.

Create a new Go file named :file:`iam_createuser.go`. You must import the relevant Go and
|sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_createuser.go
   :lines: 15-24

The code takes the new user name as an argument, and then calls ``GetUser`` with the user name.

.. literalinclude:: example_code/iam/iam_createuser.go
   :lines: 28,31-40

If you receive a ``NoSuchEntity`` error, call ``CreateUser`` because the user doesn't exist.

.. literalinclude:: example_code/iam/iam_createuser.go
   :lines: 42-56

.. _iam-example-list-user:

List |IAM| Users in Your Account
================================

You can get a list of the users in your account and print the list to the console.

Create a new Go file named :file:`iam_listusers.go`. You must import the relevant Go and
|sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_listusers.go
   :lines: 15-24

Set up a new |IAM| client.

.. literalinclude:: example_code/iam/iam_listusers.go
   :lines: 27,30-35

Call ``ListUsers`` and print the results.

.. literalinclude:: example_code/iam/iam_listusers.go
   :lines: 37-52

.. _iam_example_update_user:

Update a User's Name
====================

In this example, you change the name of an IAM user to a new value.

Create a new Go file named :file:`iam_updateuser.go`. You must import the relevant Go and|sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_updateuser.go
   :lines: 15-23

Set up a new IAM client.

.. literalinclude:: example_code/iam/iam_updateuser.go
   :lines: 27,30-35

Call ``UpdateUser``, passing in the original user name and the new name, and print the results.

.. literalinclude:: example_code/iam/iam_updateuser.go
   :lines: 37-48

.. _iam_example_delete_user:

Delete an IAM User
==================

In this example, you delete an |IAM| user.

Create a new Go file named :file:`iam_updateuser.go`. You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_updateuser.go
   :lines: 15-24

Set up a new |IAM| client.

.. literalinclude:: example_code/iam/iam_updateuser.go
   :lines: 28,31-40

Call ``UpdateUser``, passing in the user name, and print the results. If the user doesn't exist, log an error.

.. literalinclude:: example_code/iam/iam_updateuser.go
   :lines: 43-52

