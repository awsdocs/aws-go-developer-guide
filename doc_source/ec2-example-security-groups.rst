.. Copyright 2010-2020 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-ec2-work-with-security-groups:

#####################################
Working with Security Groups in |EC2|
#####################################

.. meta::
   :description: Work with security groups in |EC2| using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, |EC2| security groups

These Go examples show you how to:

* Retrieve information about your security groups
* Create a security group to access an |EC2| instance
* Delete an existing security group

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <ec2>` repository on GitHub.

.. _scenario-security-groups:

Scenario
========

An |EC2| security group acts as a virtual firewall that controls the traffic for one or
more instances. You add rules to each security group to allow traffic to or from its
associated instances. You can modify the rules for a security group at any time; the
new rules are automatically applied to all instances that are associated with the security
group.

The code in this example uses the |sdk-go| to perform these tasks by using these
methods of the |EC2| client class:

* :sdk-go-api-deep:`DescribeSecurityGroups <service/ec2/#EC2.DescribeSecurityGroups>`
* :sdk-go-api-deep:`AuthorizeSecurityGroupIngress <service/ec2/#EC2.AuthorizeSecurityGroupIngress>`
* :sdk-go-api-deep:`CreateSecurityGroup <service/ec2/#EC2.CreateSecurityGroup>`
* :sdk-go-api-deep:`DescribeVpcs <service/ec2/#EC2.DescribeVpcs>`
* :sdk-go-api-deep:`DeleteSecurityGroup <service/ec2/#EC2.DeleteSecurityGroup>`

.. _scenario-security-groups-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with |EC2| security groups. To learn more, see
  :ec2-ug:`Amazon EC2 Amazon Security Groups for Linux Instances <using-network-security>` in
  the |EC2-ug| or :ec2-ug-win:`Amazon EC2 Amazon Security Groups for Windows Instances <using-network-security>`
  in the |EC2-ug-win|.

.. _ec2-describe-security-groups:

Describing Your Security Groups
===============================

This example describes the security groups by IDs that are passed into the routine. It takes
a space separated list of group IDs as input.

To get started, create a new Go file named :file:`ec2_describe_security_groups.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/ec2/ec2_describe_security_groups.go
   :lines: 15-26

In the ``main`` function, get the security group ID that is passed in.

.. literalinclude:: example_code/ec2/ec2_describe_security_groups.go
   :lines: 33-38

Initialize a session and create an EC2 service client.

.. literalinclude:: example_code/ec2/ec2_describe_security_groups.go
   :dedent: 4
   :lines: 42-47

Obtain and print out the security group descriptions. You will explicitly check for
errors caused by an invalid group ID.

.. literalinclude:: example_code/ec2/ec2_describe_security_groups.go
   :dedent: 4
   :lines: 50-68

The following utility function is used by this example to display errors.

.. literalinclude:: example_code/ec2/ec2_describe_security_groups.go
   :lines: 71-74

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/ec2_describe_security_groups.go>`_
on GitHub.

.. _create-security-group:

Creating a Security Group
=========================

You can create new |EC2| security groups. To do this, you use the
:sdk-go-api-deep:`CreateSecurityGroup <service/ec2/#EC2.CreateSecurityGroup>` method.

This example creates a new security group with the given name and description for access to
open ports 80 and 22. If a VPC ID is not provided, it associates the security group with the
first VPC in the account.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: ec2.go.create_security_group.imports.txt
   :dedent: 0
   :language: go

Get the parameters (name, description, and optional ID of the VPC) that are passed in to the
routine.

.. literalinclude:: ec2.go.create_security_group.vars.txt
   :dedent: 4
   :language: go

Create a session and |EC2| client.

.. literalinclude:: ec2.go.create_security_group.session.txt
   :dedent: 4
   :language: go

Create an |EC2| client.              
If the VPC ID was not provided, retrieve the first one in the account.

.. literalinclude:: ec2.go.create_security_group.vpcid.txt
   :dedent: 4
   :language: go

Create the security group with the VPC ID, name, and description.

.. literalinclude:: ec2.go.create_security_group.create.txt
   :dedent: 4
   :language: go

Add permissions to the security group.

.. literalinclude:: ec2.go.create_security_group.permissions.txt
   :dedent: 4
   :language: go

The following utility function is used by this example.

.. literalinclude:: ec2.go.create_security_group.exit.txt
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/ec2_create_security_group.go>`_
on GitHub.

.. _delete-security-group:

Deleting a Security Group
=========================

You can delete an |EC2| security group in code. To do this, you use the
:sdk-go-api-deep:`DeleteSecurityGroup <service/ec2/#EC2.DeleteSecurityGroup>` method.

This example deletes a security group with the given group ID.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/ec2/ec2_delete_security_group.go
   :lines: 15-26

Get the group ID that is passed in to the routine.

.. literalinclude:: example_code/ec2/ec2_delete_security_group.go
   :lines: 32-37

Create a session.

.. literalinclude:: example_code/ec2/ec2_delete_security_group.go
   :dedent: 4
   :lines: 41-43

Then delete the security group with the group ID that is passed in.

.. literalinclude:: example_code/ec2/ec2_delete_security_group.go
   :lines: 49-65

This example uses the following utility function.

.. literalinclude:: example_code/ec2/ec2_delete_security_group.go
   :lines: 67-70

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/ec2_delete_security_group.go>`_
on GitHub.
