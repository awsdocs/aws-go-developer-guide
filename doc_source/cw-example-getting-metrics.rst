.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-cw-getting-metrics:

#################################
Getting Metrics from |CW| with Go
#################################

.. meta::
   :description: Use this code example to learn how to retrieve a list of published CloudWatch metrics and publish data points to CloudWatch metrics in Go.
   :keywords: AWS SDK for Go examples, CloudWatch, alarm actions

These Go examples show you how to retrieve a list of published |CW| metrics and publish
data points to |CW| metrics with the |sdk-go|, as follows:

* Listing metrics
* Submitting custom metrics

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <cloudwatch>` repository on GitHub.

.. _cw-get-metrics-scenario:

The Scenario
============

Metrics are data about the performance of your systems. You can enable detailed monitoring of
some resources, such as your |EC2| instances, or your own application metrics.

In this example, Go code is used to get metrics from |CW| and to send events to |CWE|.
The code uses the |sdk-go| to get metrics from |CW| by using these methods of the |CW| type:

* :sdk-go-api-deep:`ListMetrics <service/cloudwatch/#CloudWatch.ListMetrics>`
* :sdk-go-api-deep:`PutMetricData <service/cloudwatch/#CloudWatch.PutMetricData>`

.. _cw-get-metrics-prerequisites:

Prerequisites
=============

* You have  :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with |CW| metrics. To learn more,
  see :cw-dg:`Using Amazon CloudWatch Metrics <working_with_metrics>` in the |CW-ug|.

.. _cw-example-list-metrics:

Listing Metrics
===============

Create a new Go file named :file:`listing_metrics.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/cloudwatch/listing_metrics.go
   :lines: 15-23

Initialize a session that the SDK will use to load configuration, credentials, and
region information from the shared config file, ~/.aws/config, and create a new |EC2| service client.

.. literalinclude:: example_code/cloudwatch/listing_metrics.go
   :lines: 27-34

Call ``ListMetrics``, supplying the metric name, namespace, and dimensions. Print the metrics returned in the result.

.. literalinclude:: example_code/cloudwatch/listing_metrics.go
   :lines: 37-53


.. _cw-example-custom-metrics:

Submitting Custom Metrics
=========================

Create a new Go file named :file:`custom_metrics.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/cloudwatch/custom_metrics.go
   :lines: 15-23

Initialize a session that the SDK will use to load configuration, credentials, and
region information from the shared config file, ~/.aws/config, and create a new |EC2| service client.

.. literalinclude:: example_code/cloudwatch/custom_metrics.go
   :lines: 27-34

Call ``PutMetricData``, suppylying the metric name, unit, value, and dimensions. Print
any errors, or a success message.

.. literalinclude:: example_code/cloudwatch/custom_metrics.go
   :lines: 37-60

