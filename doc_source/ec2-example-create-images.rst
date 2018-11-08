.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-ec2-tags-no-block:

###########################################################
Creating |EC2| Instances with Tags or without Block Devices
###########################################################

.. meta::
   :description: Create Amazon EC2 instances with tags or no block devices using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, EC2, tags, block device


This Go example shows you how to:

*  Create an |EC2| instance with tags or set up an instance without a block device

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <ec2>` repository on GitHub.

.. _ec2-tags-scenario:

Scenarios
=========

In these examples, you use a series of Go routines to create |EC2| instances
with tags or set up an instance without a block device.

The routines use the |sdk-go| to perform these tasks by using these
methods of the :sdk-go-api-deep:`EC2 <service/ec2/#EC2>` type:

* :sdk-go-api-deep:`BlockDeviceMapping <service/ec2/#BlockDeviceMapping>`
* :sdk-go-api-deep:`RunInstances <service/ec2/#EC2.RunInstances>`
* :sdk-go-api-deep:`CreateTags <service/ec2/#EC2.CreateTags>`

.. _ec2-tags-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>`
  the |sdk-go|.

.. _create-an-instance-with-tags:

Create an Instance with Tags
============================

The |EC2| service has an operation for creating instances
(:sdk-go-api-deep:`RunInstances <service/ec2/#EC2.RunInstances>`) and another for
attaching tags to instances (:sdk-go-api-deep:`CreateTags <service/ec2/#EC2.CreateTags>`).
To create an instance with tags, call both of these
operations in succession. The following example creates an instance and then
adds a ``Name`` tag to it. The |EC2| console displays the
value of the ``Name`` tag in its list of instances.

.. literalinclude:: example_code/ec2/create_instance.go
   :lines: 15-

You can add up to 10 tags to an instance in a
single ``CreateTags`` operation.

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/create_instance.go>`_
on GitHub.

.. _create-image-without-block-device:

Create an Image without a Block Device
======================================

Sometimes when you create an |EC2| image, you might want to explicitly exclude certain block devices. To
do this, you can use the ``NoDevice``
parameter in :sdk-go-api-deep:`BlockDeviceMapping <service/ec2/#BlockDeviceMapping>`.
When this parameter is set to an empty string ``""``, the named device isn't mapped.

The ``NoDevice`` parameter is compatible only with ``DeviceName``, not with any other
field in ``BlockDeviceMapping``. The request
will fail if other parameters are present.

.. literalinclude:: example_code/ec2/create_image_no_block_device.go
   :lines: 15-

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/create_image_no_block_device.go>`_
on GitHub.
