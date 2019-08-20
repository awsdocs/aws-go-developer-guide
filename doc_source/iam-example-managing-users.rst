.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-iam-users:

####################
Managing |IAM| Users
####################

.. meta::
   :description: Learn to manage IAM users with this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, IAM users


This Go example shows you how to create, update, view, and
delete |IAM| users. You can download complete versions of these
example files from the :doc-examples-go:`aws-doc-sdk-examples <s3>` repository on GitHub.

.. _iam-users-scenario:

Scenario
========

In this example, you use a series of Go routines to manage users in |IAM|.
The routines use the |sdk-go| IAM client methods that follow:

* :sdk-go-api-deep:`CreateUser <service/iam/#IAM.CreateUser>`
* :sdk-go-api-deep:`ListUsers <service/iam/#IAM.ListUsers>`
* :sdk-go-api-deep:`UpdateUser <service/iam/#IAM.UpdateUser>`
* :sdk-go-api-deep:`GetUser <service/iam/#IAM.GetUser>`
* :sdk-go-api-deep:`DeleteUser <service/iam/#IAM.DeleteUser>`
* :sdk-go-api-deep:`GetAccountAuthorizationDetails <service/iam/#IAM.GetAccountAuthorizationDetails>`

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

Create a new Go file named :file:`iam_deleteuser.go`. You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_deleteuser.go
   :lines: 17-25

Set up a new |IAM| client.

.. literalinclude:: example_code/iam/iam_deleteuser.go
   :lines: 32-34, 37

Call ``DeleteUser``, passing in the user name, and print the results. If the user doesn't exist, display an error.

.. literalinclude:: example_code/iam/iam_deleteuser.go
   :lines: 39-52

.. _iam_example_get_admins:

List the IAM Users who have Administrator Privileges
====================================================

In this example, you list the |IAM| users who have administrator privileges
(a policy or attached policy of the user or a group to which the user belongs
has the name **AdministratorAccess**.

Create a new Go file named :file:`IamGetAdmins.go`.
Import the following packages.

.. literalinclude:: example_code/iam/IamListAdmins.go
   :lines: 17-24
   :dedent: 0
   :language: go

Create a method to determine whether a user has a policy that
has administrator privileges.

.. literalinclude:: example_code/iam/IamListAdmins.go
   :lines: 26-34
   :dedent: 0
   :language: go

Create a method to determine whether a user has an attached policy that
has administrator privileges.

.. literalinclude:: example_code/iam/IamListAdmins.go
   :lines: 36-44
   :dedent: 0
   :language: go

Create a method that determines whether a group has a policy that
has adminstrator privileges.

.. literalinclude:: example_code/iam/IamListAdmins.go
   :lines: 46-65
   :dedent: 0
   :language: go

Create a method that determines whether a group has an attached policy that
has administrator privileges.

.. literalinclude:: example_code/iam/IamListAdmins.go
   :lines: 67-83
   :dedent: 0
   :language: go

Create a method that determines whether any group to which the user belongs
has administrator privileges.

.. literalinclude:: example_code/iam/IamListAdmins.go
   :lines: 85-109
   :dedent: 0
   :language: go

Create a method that determines whether a user has administrator privileges.

.. literalinclude:: example_code/iam/IamListAdmins.go
   :lines: 111-129
   :dedent: 0
   :language: go

Create a main method with an IAM client in :code:`us-west-2`.
Create variables to keep track of how many users we have
and how many of those have administrator privileges.

.. literalinclude:: example_code/iam/IamListAdmins.go
   :lines: 131-142
   :dedent: 0
   :language: go

Create the input for and call :code:`GetAccountAuthorizationDetails`.
If there is an error, print an error message and quit.

.. literalinclude:: example_code/iam/IamListAdmins.go
   :lines: 145-152
   :dedent: 4
   :language: go

Loop through the users.
If a user has adminstrator privileges,
print their name and increment the number of users
who have adminstrator privileges.

.. literalinclude:: example_code/iam/IamListAdmins.go
   :lines: 155-167
   :dedent: 4
   :language: go

If we did not get all of the users in the first call to
:code:`GetAccountAuthorizationDetails`,
loop through the next set of users and determine which
of those have adminstrator privileges.

.. literalinclude:: example_code/iam/IamListAdmins.go
   :lines: 179-190
   :dedent: 4
   :language: go

Finally, display the number of users who have
administrator access.

.. literalinclude:: example_code/iam/IamListAdmins.go
   :lines: 192-193
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/iam/IamListAdmins.go>`_
on GitHub.
