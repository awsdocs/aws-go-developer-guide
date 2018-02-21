.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-iam-policies:

###########################
Working with |IAM| Policies
###########################

.. meta::
   :description: Learn to manage IAM policies using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, IAM policies


This Go example shows you how to create, get, attach, and detach |IAM| policies. You can download complete versions of these example files from the :doc-examples-go:`aws-doc-sdk-examples <s3>` repository on GitHub.

.. _iam-policies-scenario:

Scenario
========

You grant permissions to a user by creating a policy, which is a document that lists the actions that a user can perform and the resources those actions can affect. Any actions or resources that are not explicitly allowed are denied by default. Policies can be created and attached to users, groups of users, roles
assumed by users, and resources.

In this example, you use a series of Go routines to manage policies in |IAM|. The routines use the |sdk-go| IAM client methods that follow:

* :sdk-go-api-deep:`CreatePolicy <service/iam/#IAM.CreatePolicy>`
* :sdk-go-api-deep:`GetPolicy <service/iam/#IAM.GetPolicy>`
* :sdk-go-api-deep:`ListAttachedRolePolicies <service/iam/#IAM.ListAttachedRolePolicies>`
* :sdk-go-api-deep:`AttachRolePolicy <service/iam/#IAM.AttachRolePolicy>`
* :sdk-go-api-deep:`DetachRolePolicy <service/iam/#IAM.DetachRolePolicy>`

.. _iam-policies-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with |IAM| policies. To learn more, see :iam-ug:`Overview of Access Management: Permissions and Policies <introduction_access-management>` in the |IAM-ug|.

.. _iam-example-create-policy:

Create an IAM Policy
====================

This code creates a new IAM Policy. Create a new Go file named :file:`iam_createpolicy.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_createpolicy.go
   :lines: 15-24

Define two structs. The first is the definition of the policies to upload to |IAM|.

.. literalinclude:: example_code/iam/iam_createpolicy.go
   :lines: 27-30

The second dictates what this policy will allow or disallow.

.. literalinclude:: example_code/iam/iam_createpolicy.go
   :lines: 33-37

Set up the session and |IAM| client.

.. literalinclude:: example_code/iam/iam_createpolicy.go
   :lines: 41,44-49

Build the policy document using the structures defined earlier.

.. literalinclude:: example_code/iam/iam_createpolicy.go
   :lines: 52-75

Marshal the policy to JSON and pass to ``CreatePolicy``.

.. literalinclude:: example_code/iam/iam_createpolicy.go
   :lines: 77-94

.. _iam-example-get-policy:

Get an IAM Policy
=================

In this example, you retrieve an existing policy from |IAM|. Create a new Go file named :file:`iam_getpolicy.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_getpolicy.go
   :lines: 15-22

Set up a new |IAM| client.

.. literalinclude:: example_code/iam/iam_getpolicy.go
   :lines: 26,29-34

Call ``GetPolicy``, passing in the ARN for the policy (which is hard coded in this example), and print the results.

.. literalinclude:: example_code/iam/iam_getpolicy.go
   :lines: 34-47

.. _iam-example-attach-managed-role-policy:

Attach a Managed Role Policy
============================

In this example, you attach an |IAM| managed role policy. Create a new Go file named :file:`iam_attachuserpolicy.go`. You'll call the ``ListAttachedRolePolicies`` method of the |IAM| service object, which returns an array of managed policies.

Then, you'll check the array members to see if the policy you want to attach to the role is already attached. If the policy isn't attached, you'll call the ``AttachRolePolicy`` method to attach it.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_attachuserpolicy.go
   :lines: 15-23

Set up a new |IAM| client.

.. literalinclude:: example_code/iam/iam_attachuserpolicy.go
   :lines: 27,30-35

Declare variables to hold the name and ARN of the policy.

.. literalinclude:: example_code/iam/iam_attachuserpolicy.go
   :lines: 37-39

Paginate through all the role policies. If your role exists on any role policy, you set the ``pageErr`` and return ``false``, stopping the pagination.

.. literalinclude:: example_code/iam/iam_attachuserpolicy.go
   :lines: 44-61

If your role policy is not attached already, call ``AttachRolePolicy``.

.. literalinclude:: example_code/iam/iam_attachuserpolicy.go
   :lines: 63-83

.. _iam-example-detach-managed-role-policy:

Detach a Managed Role Policy
============================

In this example, you detach a role policy. Once again, you call the ``ListAttachedRolePolicies`` method of the |IAM| service object, which returns an array of managed policies.

Then, check the array members to see if the policy you want to detach from the role is attached. If the policy is attached, call the ``DetachRolePolicy`` method to detach it.

Create a new Go file named :file:`iam_detachuserpolicy.go`. You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_detachuserpolicy.go
   :lines: 15-23

Set up a new |IAM| client.

.. literalinclude:: example_code/iam/iam_detachuserpolicy.go
   :lines: 27,30-35

Declare variables to hold the name and ARN of the policy.

.. literalinclude:: example_code/iam/iam_detachuserpolicy.go
   :lines: 37-39

Paginate through all the role policies. If the role exists on any role policy,
you stop iterating and detach the role.

.. literalinclude:: example_code/iam/iam_detachuserpolicy.go
   :lines: 43-81

