.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-cw-getting-metrics:

#########################
Getting Metrics from |CW|
#########################

.. meta::
   :description: Retrieve a list of published CloudWatch metrics and publish data points
                 to CloudWatch metrics using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, CloudWatch, alarm actions

These Go examples show you how to retrieve a list of published |CW| metrics and publish
data points to |CW| metrics with the |sdk-go|, as follows:

* Listing metrics
* Submitting custom metrics

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <cloudwatch>` repository on GitHub.

.. _cw-get-metrics-scenario:

Scenario
========

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

List Metrics
============

Choose **Copy** to save the code locally.

Create the file :file:`listing_metrics.go`.

Import the packages used in the example.

.. literalinclude:: example_code/cloudwatch/listing_metrics.go
   :lines: 17-24
   :dedent: 0
   :language: go

Get the metric name, namespace, and dimension name from the command line.

.. literalinclude:: example_code/cloudwatch/listing_metrics.go
   :lines: 27-34
   :dedent: 4
   :language: go

Initialize a session that the SDK will use to load credentials
from the shared credentials file ~/.aws/credentials,
load your configuration from the shared configuration file ~/.aws/config,
and create a |CW| client.

.. literalinclude:: example_code/cloudwatch/listing_metrics.go
   :lines: 39-44
   :dedent: 4
   :language: go

Call :code:`ListMetrics`, supplying the metric name, namespace, and dimension name. Print the metrics returned in the result.

.. literalinclude:: example_code/cloudwatch/listing_metrics.go
   :lines: 47-55, 61
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudwatch/listing_metrics.go>`_
on GitHub.

.. _cw-example-custom-metrics:

Submit Custom Metrics
=====================

Choose **Copy** to save the code locally.

Create the file :file:`custom_metrics.go`.

Import the packages used in the example.

.. literalinclude:: example_code/cloudwatch/custom_metrics.go
   :lines: 17-23
   :dedent: 0
   :language: go

Initialize a session that the SDK will use to load credentials
from the shared credentials file ~/.aws/credentials,
load your configuration from the shared configuration file ~/.aws/config,
and create a |CW| client.

.. literalinclude:: example_code/cloudwatch/custom_metrics.go
   :lines: 29-34
   :dedent: 4
   :language: go

Call :code:`PutMetricData` with the
the custom namespace "Site/Traffic".
The namespace has two custom dimensions: "SiteName" and "PageURL".
"SiteName" has the value "example.com",
the "UniqueVisitors" value 5885 and the "UniqueVisits" value 8628.
"PageURL" has the value "my-page.html",
and a "PageViews" value 18057.

.. literalinclude:: example_code/cloudwatch/custom_metrics.go
   :lines: 36-73
   :dedent: 4
   :language: go

If there are any errors, print them out,
otherwise list some information about the custom metrics.

.. literalinclude:: example_code/cloudwatch/custom_metrics.go
   :lines: 74-95
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudwatch/custom_metrics.go>`_
on GitHub.
