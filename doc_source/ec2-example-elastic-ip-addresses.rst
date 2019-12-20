.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-ec2-allocate-address:

###################################
Using Elastic IP Addresses in |EC2|
###################################

.. meta::
   :description: Allocate Elastic IP addresses in Amazon EC2 using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, Elastic IP addresses


These Go examples show you how to:

* Describe |EC2| instance IP addresses
* Allocate addresses to |EC2| instances
* Release |EC2| instance IP addresses

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <ec2>` repository on GitHub.

.. _ec-allocate-address-scenario:

Scenario
========

An Elastic IP address is a static IP address designed for dynamic cloud computing that is associated with
your AWS account. It is a public IP address, reachable from
the Internet. If your instance doesn't have a public IP address, you can associate an Elastic IP
address with the instance to enable communication with the Internet.

In this example, you use a series of Go routines to perform several |EC2| operations involving
Elastic IP addresses. The routines use the |sdk-go| to manage Elastic IP addresses by using these
methods of the |EC2| client class:

* :sdk-go-api-deep:`DescribeAddresses <service/ec2/#EC2.DescribeAddresses>`
* :sdk-go-api-deep:`AllocateAddress <service/ec2/#EC2.AllocateAddress>`
* :sdk-go-api-deep:`AssociateAddress <service/ec2/#EC2.AssociateAddress>`
* :sdk-go-api-deep:`ReleaseAddress <service/ec2/#EC2.ReleaseAddress>`

.. _ec-allocate-address-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with Elastic IP addresses in |EC2|. To learn more, see
  :ec2-ug:`Elastic IP Addresses <elastic-ip-addresses-eip>` in the |EC2-ug| or
  :ec2-ug-win:`Elastic IP Addresses <elastic-ip-addresses-eip>` in the |EC2-ug-win|.

.. _ec2-example-describe:

Describe Instance IP Addresses
==============================

Create a new Go file named :file:`ec2_describe_addresses.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/ec2/ec2_describe_addresses.go
   :lines: 15-24

Get the Address Descriptions
----------------------------

This routine prints out the Elastic IP Addresses for the account's VPC. Initialize a session that the
SDK will use to load credentials from the shared credentials file,
~/.aws/credentials, and create a new EC2 service client.

.. literalinclude:: example_code/ec2/ec2_describe_addresses.go
   :lines: 30, 33-38

Make the API request to EC2 filtering for the addresses in the account's VPC.

.. literalinclude:: example_code/ec2/ec2_describe_addresses.go
   :lines: 42-63

The ``fmtAddress`` and ``exitErrorf`` functions are utility functions used in the example.

.. literalinclude:: example_code/ec2/ec2_describe_addresses.go
   :lines: 65-

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/ec2_describe_addresses.go>`_
on GitHub.

.. _ec2-example-allocate:

Allocate Addresses to Instances
===============================

Create a new Go file named :file:`ec2_allocate_address.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/ec2/ec2_allocate_address.go
   :lines: 15-25

This routine attempts to allocate a VPC Elastic IP address for the current region. The IP address
requires and will be associated with the instance ID that is passed in.


.. literalinclude:: example_code/ec2/ec2_allocate_address.go
   :lines: 32-37

You will need to initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials, and create a new |EC2| service client.

.. literalinclude:: example_code/ec2/ec2_allocate_address.go
   :lines: 41-46

Call :sdk-go-api-deep:`AllocateAddress <service/ec2/#EC2.AllocateAddress>`, passing in
"vpc" as the Domain value.

.. literalinclude:: example_code/ec2/ec2_allocate_address.go
   :lines: 49-54

Call :sdk-go-api-deep:`AssociateAddress <service/ec2/#EC2.AssociateAddress>` to associate the new Elastic IP
address with an existing |EC2| instance, and print out the results.

.. literalinclude:: example_code/ec2/ec2_allocate_address.go
   :lines: 57-68

This example also uses the ``exitErrorf`` utility function.

.. literalinclude:: example_code/ec2/ec2_allocate_address.go
   :lines: 70-73

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/ec2_allocate_address.go>`_
on GitHub.           

.. _ec2-example-release-instance-addresses:           

Release Instance IP Addresses
=============================

This routine releases an Elastic IP address allocation ID. If the address is associated
with an |EC2| instance, the association is removed.

Create a new Go file named :file:`ec2_release_address.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/ec2/ec2_release_address.go
   :lines: 15-26

The routine requires that the user pass in the allocation ID of the Elastic IP address.

.. literalinclude:: example_code/ec2/ec2_release_address.go
   :lines: 33-38

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials, and create a new EC2 service client.

.. literalinclude:: example_code/ec2/ec2_release_address.go
   :lines: 42-47

Attempt to release the Elastic IP address by using the allocation ID.

.. literalinclude:: example_code/ec2/ec2_release_address.go
   :lines: 50-62

This example uses the ``fmtAddress`` and ``exitErrorf`` utility functions.

.. literalinclude:: example_code/ec2/ec2_describe_addresses.go
   :lines: 65-

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/ec2_describe_addresses.go>`_
on GitHub.
