.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-cw-describe-alarms:

######################
Describing |CW| Alarms
######################

.. meta::
   :description: Learn to describe alarms in Amazon Cloudwatch using the AWS SDK for Go.
   :keywords: AWS SDK for Go code examples, CloudWatch, alarms

This example shows you how to retrieve basic information that describes your |CW| alarms.

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <cloudwatch>` repository on GitHub.

.. _cw-describe-alarms-scenario:

Scenario
========

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

Choose **Copy** to save the code locally.

Create the file :file:`describe_alarms.go`.
Import the packages used in the example.

.. literalinclude:: example_code/cloudwatch/describe_alarms.go
   :lines: 16-23
   :dedent: 0
   :language: go

Initialize a session that the SDK will use to load credentials
from the shared credentials file ~/.aws/credentials,
load your configuration from the shared configuration file ~/.aws/config,
and create a |CW| client.

.. literalinclude:: example_code/cloudwatch/describe_alarms.go
   :lines: 28-32
   :dedent: 4
   :language: go

Call :code:`DescribeAlarms`, and print the results.

.. literalinclude:: example_code/cloudwatch/describe_alarms.go
   :lines: 34, 41-43
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudwatch/describe_alarms.go>`_
on GitHub.
