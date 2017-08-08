.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-cw-describe-alarms:

########################################
Describing |CW| Alarms with the |sdk-go|
########################################

.. meta::
   :description: Use this code example to learn how to describe alarms in Amazon Cloudwatch.
   :keywords: AWS SDK for Go examples, CloudWatch, alarms

This example shows you how to retrieve basic information that describes your |CW| alarms.

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <cloudwatch>` repository on GitHub.

.. _cw-describe-alarms-scenario:

The Scenario
============

An alarm watches a single metric over a time period you specify. The alarm performs one or more actions
based on the value of the metric relative to a given threshold over a number of time periods.

In this example, Go code is used to describe alarms in |CW|. The code uses the
|sdk-go| to describe alarms by using this method of the ``AWS.CloudWatch`` client class:

* :sdk-go-api-deep:`DescribeAlarms <service/cloudwatch/#CloudWatch.DescribeAlarms>`

.. _cw-describe-alarms-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with |CW| alarms. To learn more, see
  :cw-dg:`Creating Amazon CloudWatch Alarms <AlarmThatSendsEmail>` in the |CW-ug|.

.. _cw-example-alarms:

Describe Alarms
===============

Initialize a session that the SDK will use to load configuration, credentials, and region information
from the shared config file, ~/.aws/config, and create a new |EC2| service client.

.. literalinclude:: example_code/cloudwatch/describe_alarms.go
   :lines: 15-19

Set up the ``DescribeAlarmsInput`` structure, pass it in to the ``DescribeAlarms`` method, and print the results.

.. literalinclude:: example_code/cloudwatch/describe_alarms.go
   :lines: 21-36,39-41,45



