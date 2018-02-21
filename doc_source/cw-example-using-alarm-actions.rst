.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-cw-alarms:

######################################
Using Alarms and Alarm Actions in |CW|
######################################

.. meta::
   :description: Create alarms and alarm actions to change the state of EC2 instances using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, CloudWatch, alarm actions

These Go examples show you how to change the state of your |EC2| instances automatically based
on a |CW| alarm, as follows:

* Creating and enabling actions on an alarm
* Disabling actions on an alarm

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <cloudwatch>` repository on GitHub.

.. _cw-create-alarm-actions-scenario:

Scenario
========

You can use alarm actions to create alarms that automatically stop, terminate, reboot, or
recover your |EC2| instances. You can use the stop or terminate actions when you no
longer need an instance to be running. You can use the reboot and recover actions to
automatically reboot the instance.

In this example, Go code is used to define an alarm action in |CW| that triggers the reboot
of an |EC2| instance. The code uses the |sdk-go| to manage instances by using these methods of
:sdk-go-api-deep:`PutMetricAlarm <service/cloudwatch/#CloudWatch>` type:

* :sdk-go-api-deep:`PutMetricAlarm <service/cloudwatch/#CloudWatch.PutMetricAlarm>`
* :sdk-go-api-deep:`EnableAlarmActions <service/cloudwatch/#CloudWatch.EnableAlarmActions>`
* :sdk-go-api-deep:`DisableAlarmActions <service/cloudwatch/#CloudWatch.DisableAlarmActions>`

.. _cw-create-alarm-actions-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with |CW| alarm actions. To learn more, see
  :cw-dg:`Create Alarms to Stop, Terminate, Reboot, or Recover an Instance <UsingAlarmActions>`
  in the |CW-ug|.

.. _cw-example-alarm-actions:

Create and Enable Actions on an Alarm
=====================================

Choose **Copy** to save the code locally.

Create the file :file:`create_enable_alarms.go`.

Import packages used in the example.

.. literalinclude:: example_code/cloudwatch/create_enable_alarms.go
   :lines: 16-24
   :dedent: 0
   :language: go

Get an instance name, value, and alarm name.

.. literalinclude:: example_code/cloudwatch/create_enable_alarms.go
   :lines: 27-34
   :dedent: 4
   :language: go

Initialize a session that the SDK will use to load credentials
from the shared credentials file ~/.aws/credentials,
load your configuration from the shared configuration file ~/.aws/config,
and create a |CW| client.

.. literalinclude:: example_code/cloudwatch/create_enable_alarms.go
   :lines: 39-44
   :dedent: 4
   :language: go

Create a metric alarm that reboots an instance if its CPU utilization is greater
than 70 percent.

.. literalinclude:: example_code/cloudwatch/create_enable_alarms.go
   :lines: 48-72
   :dedent: 4
   :language: go

Call :code:`EnableAlarmActions` with the new alarm for the instance,
and display a message.

.. literalinclude:: example_code/cloudwatch/create_enable_alarms.go
   :lines: 79-83, 89
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudwatch/create_enable_alarms.go>`_
on GitHub.

Disable Actions on an Alarm
===========================

Choose **Copy** to save the code locally.

Create the file :file:`disable_alarm.go`.

Import the packages used in the example.

.. literalinclude:: example_code/cloudwatch/disable_alarm.go
   :lines: 17-23
   :dedent: 0
   :language: go

Get the name of the alarm from the command line.

.. literalinclude:: example_code/cloudwatch/disable_alarm.go
   :lines: 26-31
   :dedent: 4
   :language: go

Initialize a session that the SDK will use to load credentials
from the shared credentials file ~/.aws/credentials,
load your configuration from the shared configuration file ~/.aws/config,
and create a |CW| client.

.. literalinclude:: example_code/cloudwatch/disable_alarm.go
   :lines: 36-41
   :dedent: 4
   :language: go

Call :code:`DisableAlarmActions` to disable the actions for this alarm
and display a message.

.. literalinclude:: example_code/cloudwatch/disable_alarm.go
   :lines: 44-48, 54
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudwatch/disable_alarm.go>`_
on GitHub.
