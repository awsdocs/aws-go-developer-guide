.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-iam-aliases:

##############################
Managing |IAM| Account Aliases
##############################

.. meta::
   :description: Learn to manage IAM account aliases using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, IAM alias


This Go example shows you how to create, list, and delete |IAM| account aliases. You can download complete versions of these example files from the :doc-examples-go:`aws-doc-sdk-examples <s3>` repository on GitHub.

.. _iam-aliases-scenario:

Scenario
========

You can use a series of Go routines to manage aliases in |IAM|. The routines use the |sdk-go| IAM client methods that follow:

* :sdk-go-api-deep:`CreateAccountAlias <service/iam/#IAM.CreateAccountAlias>`
* :sdk-go-api-deep:`ListAccountAliases <service/iam/#IAM.ListAccountAliases>`
* :sdk-go-api-deep:`DeleteAccountAlias <service/iam/#IAM.DeleteAccountAlias>`

.. _iam-aliases-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with |IAM| account aliases. To learn more, see :iam-ug:`Your AWS Account ID and Its Alias <console_account-alias>` in the |IAM-ug|.


.. _iam-example-create-alias:

Create a New IAM Account Alias
==============================

.. Intro para for this should mention alias, no? Maybe an example of when/why you'd create the new alias? and should first sentence below say "new IAM account alias"?

This code creates a new |IAM| user.

Create a new Go file named :file:`iam_createaccountalias.go`. You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_createaccountalias.go
   :lines: 15-23

Set up a session and an |IAM| client.

.. literalinclude:: example_code/iam/iam_createaccountalias.go
   :lines: 28,31-40

The code takes the new alias as an argument, and then calls ``CreateAccountAlias`` with the alias name.

.. literalinclude:: example_code/iam/iam_createaccountalias.go
   :lines: 37-47

.. _iam-example-list-aliases:

List |IAM| Account Aliases
==========================

This code lists the aliases for your |IAM| account.

Create a new Go file named :file:`iam_listaccountaliases.go`. You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_listaccountaliases.go
   :lines: 15-23

Set up a session and an |IAM| client.

.. literalinclude:: example_code/iam/iam_listaccountaliases.go
   :lines: 27,30-35

The code calls ``ListAccountAliases``, specifying to return a maximum of 10 items.

.. literalinclude:: example_code/iam/iam_listaccountaliases.go
   :lines: 37-52

.. _iam-example-delete-aliases:

Delete an |IAM| Account Alias
=============================

This code deletes a specified |IAM| account alias.

Create a new Go file with the name :file:`iam_deleteaccountalias.go`. You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_deleteaccountalias.go
   :lines: 15-23

Set up a session and an |IAM| client.

.. literalinclude:: example_code/iam/iam_deleteaccountalias.go
   :lines: 27,30-35

The code calls ``ListAccountAliases``, specifying to return a maximum of 10 items.

.. literalinclude:: example_code/iam/iam_deleteaccountalias.go
   :lines: 37-47


