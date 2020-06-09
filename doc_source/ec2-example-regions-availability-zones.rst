.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-ec2-regions-and-azs:

###############################################
Using Regions and Availability Zones with |EC2|
###############################################

.. meta::
   :description: Work with regions and Availability Zones using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, EC2 availability zones and regions

These Go examples show you how to retrieve details about AWS Regions and Availability Zones.

The code in this example uses the |sdk-go| to perform these tasks by using these
methods of the |EC2| client class:

* :sdk-go-api-deep:`DescribeRegions <service/ec2/#EC2.DescribeRegions>`
* :sdk-go-api-deep:`DescribeAvailabilityZones <service/ec2/#EC2.DescribeAvailabilityZones>`

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <ec2>` repository on GitHub.

.. _ec2-scenario-regions-and-azs:

Scenario
========

|EC2| is hosted in multiple locations worldwide. These locations are composed of AWS Regions and Availability
Zones. Each region is a separate geographic area with multiple, isolated locations known as Availability
Zones. |EC2| provides the ability to place instances and data in these multiple locations.

In this example, you use Go code to retrieve details about regions and Availability Zones. The code uses
the |sdk-go| tomanage instances by using the following methods of the |EC2| client class:

* :sdk-go-api-deep:`DescribeAvailabilityZones <service/ec2/#EC2.DescribeAvailabilityZones>`
* :sdk-go-api-deep:`DescribeRegions <service/ec2/#EC2.DescribeRegions>`

.. _ec2-regions-and-azs-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with AWS Regions and Availability Zones. To learn more, see
  :ec2-ug:`Regions and Availability Zones <using-regions-availability-zones>` in the
  |ec2-ug| or :ec2-ug-win:`Regions and Availability Zones <resources>` in the |ec2-ug-win|.

List the Regions
================

This example lists the regions in which |EC2| is available.

To get started, create a new Go file named :file:`regions_and_availability.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/ec2/regions_and_availability.go
   :lines: 15-22

In the ``main`` function, create a session
with credentials from the shared credentials file,
~/.aws/credentials,
and create a new EC2 client.

.. literalinclude:: example_code/ec2/regions_and_availability.go
   :lines: 26-33

Print out the list of regions that work with |EC2| that are returned by calling ``DescribeRegions``.

.. literalinclude:: example_code/ec2/regions_and_availability.go
   :lines: 36-40

Add a call that retrieves Availability Zones only for the region of the EC2 service object.

.. literalinclude:: example_code/ec2/regions_and_availability.go
   :lines: 45-52

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/regions_and_availability.go>`_
on GitHub.
