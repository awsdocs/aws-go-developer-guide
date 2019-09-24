.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-ec2-manage-instances:

########################
Managing |EC2| Instances
########################

.. meta::
   :description: Learn to manage Amazon EC2 instances using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, EC2 instance management


These Go examples show you how to:

* Describe |EC2| instances
* Manage |EC2| instance monitoring
* Start and stop |EC2| instances
* Reboot |EC2| instances

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <ec2>` repository on GitHub.

.. _ec2-manage-instances-scenario:

Scenario
========

In these examples, you use a series of Go routines to perform several basic instance management operations.

The routines use the |sdk-go| to perform the operations by using these
methods of the |EC2| client class:

* :sdk-go-api-deep:`DescribeInstances <service/ec2/#EC2.DescribeInstances>`
* :sdk-go-api-deep:`MonitorInstances <service/ec2/#EC2.MonitorInstances>`
* :sdk-go-api-deep:`UnmonitorInstances <service/ec2/#EC2.UnmonitorInstances>`
* :sdk-go-api-deep:`StartInstances <service/ec2/#EC2.StartInstances>`
* :sdk-go-api-deep:`StopInstances <service/ec2/#EC2.StopInstances>`
* :sdk-go-api-deep:`RebootInstances <service/ec2/#EC2.RebootInstances>`

.. _ec2-manage-instances-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with the lifecycle of |EC2| instances. To learn more,
  see :ec2-ug:`Instance Lifecycle <ec2-instance-lifecycle>` in the |EC2-ug|.

.. _describing-your-instances:

Describe Your Instances
=======================

Create a new Go file named :file:`describing_instances.go`.

The |EC2| service has an operation for describing instances,
:sdk-go-api-deep:`DescribeInstances <service/ec2/#EC2.DescribeInstances>`.

Import the required |sdk-go| packages.

.. literalinclude:: example_code/ec2/describing_instances.go
   :lines: 15-22

Use the following code to create a session and |EC2| client.

.. literalinclude:: example_code/ec2/describing_instances.go
   :lines: 25-31
   :dedent: 4

Call ``DescribeInstances`` to get detailed information for each instance.

.. literalinclude:: example_code/ec2/describing_instances.go
   :lines: 33-39
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/describing_instances.go>`_
on GitHub.

.. _ec2-manage-instance-monitoring:

Manage Instance Monitoring
==========================

Create a new Go file named :file:`monitoring_instances.go`.

Import the required |sdk-go| packages.

.. literalinclude:: example_code/ec2/monitoring_instances.go
   :lines: 15-25

To access |EC2|, create an EC2 client.

.. literalinclude:: example_code/ec2/monitoring_instances.go
   :lines: 30-37

Based on the value of a command-line argument (ON or OFF), call either the ``MonitorInstances``
method of the |EC2| service object to begin detailed monitoring of the specified instances, or  the ``UnmonitorInstances``
method. Before you try to change the monitoring of these instances, use the ``DryRun`` parameter to test
whether you have permission
to change instance monitoring.

.. literalinclude:: example_code/ec2/monitoring_instances.go
   :lines: 40, 43-51, 54, 56-63, 65-89

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/monitoring_instances.go>`_
on GitHub.

.. _scenario-starting-stopping:

Start and Stop Instances
========================

Create a new Go file named :file:`start_stop_instances.go`.

Import the required |sdk-go| packages.

.. literalinclude:: example_code/ec2/start_stop_instances.go
   :lines: 15-25

To access |EC2|, create an EC2 client. The user will pass in a state value of START or STOP and
the instance ID.

.. literalinclude:: example_code/ec2/start_stop_instances.go
   :lines: 30-37

Based on the value of a command-line argument (START or STOP), call either the ``StartInstances``
method of the |EC2| service object to start the specified instances, or the ``StopInstances`` method
to stop them. Before you try to start or stop the selected instances, use the ``DryRun`` parameter to
test whether you have permission to start or stop them.

.. literalinclude:: example_code/ec2/start_stop_instances.go
   :lines: 40, 43-51, 54-87

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/start_stop_instances.go>`_
on GitHub.

.. _ec2-reboot-instances:

Reboot Instances
================

Create a new Go file named :file:`reboot_instances.go`.

Import the required |sdk-go| packages.

.. literalinclude:: example_code/ec2/reboot_instances.go
   :lines: 15-25

To access |EC2|, create an EC2 client. The user will pass in a state value of START or STOP and
the instance ID.

.. literalinclude:: example_code/ec2/reboot_instances.go
   :lines: 31-36

Based on the value of a command-line argument (START or STOP), call either the ``StartInstances``
method of the |EC2| service object to start the specified instances, or the ``StopInstances`` method
to stop them. Before you try to reboot the selected instances, use the ``DryRun`` parameter
to test whether the instance exists and that you have
permission to reboot it.

.. literalinclude:: example_code/ec2/reboot_instances.go
   :lines: 40-48

If the error code is ``DryRunOperation``, it means that you do have the permissions you need to
reboot the instance.

.. literalinclude:: example_code/ec2/reboot_instances.go
   :lines: 51,53-63

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ec2/reboot_instances.go>`_
on GitHub.
