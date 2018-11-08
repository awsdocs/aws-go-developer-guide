.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-ec2-key-pairs:

############################
Working with |EC2| Key Pairs
############################

.. meta::
   :description: Manage Amazon EC2 key pairs using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, EC2, key pairs


These Go examples show you how to:

* Describe an |EC2| key pair
* Create an |EC2| key pair
* Delete an |EC2| key pair

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <ec2>` repository on GitHub.

.. _ec2-key-pairs-scenario:

Scenario
========

|EC2| uses public–key cryptography to encrypt and decrypt login information. Public–key cryptography uses a
public key to encrypt data, then the recipient uses the private key to decrypt the data. The public and private
keys are known as a key pair.

The routines use the |sdk-go| to perform these tasks by using these
methods of the :sdk-go-api-deep:`EC2 <service/ec2/#EC2>` type:

* :sdk-go-api-deep:`CreateKeyPair <service/ec2/#EC2.CreateKeyPair>`
* :sdk-go-api-deep:`DeleteKeyPair <service/ec2/#EC2.DeleteKeyPair>`
* :sdk-go-api-deep:`DescribeKeyPairs <service/ec2/#EC2.DescribeKeyPairs>`

.. _ec2-key-pairs-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the SDK.
* You are familiar with |EC2| key pairs. To learn more, see
  :ec2-ug:`Amazon EC2 Key Pairs <ec2-key-pairs>` in the |EC2-ug|.

.. _example-describing-your-key-pairs:

Describe Your Key Pairs
=======================

Create a new Go file named :file:`ec2_describe_keypairs.go`.

Import the required |sdk-go| packages.

.. literalinclude:: example_code/ec2/ec2_describe_keypairs.go
   :lines: 15-23

Use the following code to create a session and |EC2| client.

.. literalinclude:: example_code/ec2/ec2_describe_keypairs.go
   :lines: 29,32-37

Call ``DescribeKeyPairs`` to get a list of key pairs and print them out.

.. literalinclude:: example_code/ec2/ec2_describe_keypairs.go
   :lines: 40-49

The routine uses the following utility function.

.. literalinclude:: example_code/ec2/ec2_describe_keypairs.go
   :lines: 51-54

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/ec2_describe_keypairs.go>`_
on GitHub.

.. _example-create-key-pair:

Create a Key Pair
=================

Create a new Go file named :file:`ec2_create_keypair.go`.

Import the required |sdk-go| packages.

.. literalinclude:: example_code/ec2/ec2_create_keypair.go
   :lines: 15-26

Get the key pair name passed in to the code and, to access |EC2|, create an EC2 client.

.. literalinclude:: example_code/ec2/ec2_create_keypair.go
   :lines: 32-37,41-46

Create a new key pair with the provided name.

.. literalinclude:: example_code/ec2/ec2_create_keypair.go
   :lines: 49-62

The routine uses the following utility function.

.. literalinclude:: example_code/ec2/ec2_create_keypair.go
   :lines: 64-67

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/ec2_create_keypair.go>`_
on GitHub.

.. _example-delete-key-pair:

Delete a Key Pair
=================

Create a new Go file named :file:`ec2_delete_keypair.go`.

Import the required |sdk-go| packages.

.. literalinclude:: example_code/ec2/ec2_delete_keypair.go
   :lines: 15-26

Get the key pair name passed in to the code and, to access |EC2|, create an EC2 client.

.. literalinclude:: example_code/ec2/ec2_delete_keypair.go
   :lines: 33-39,42-47

Delete the key pair with the provided name.

.. literalinclude:: example_code/ec2/ec2_delete_keypair.go
   :lines: 50-61

The routine uses the following utility function.

.. literalinclude:: example_code/ec2/ec2_delete_keypair.go
   :lines: 63-66

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/ec2_delete_keypair.go>`_
on GitHub.
