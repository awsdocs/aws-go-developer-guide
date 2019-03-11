.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-cloudwatch:

####################################
|CWlong| Examples Using the |sdk-go|
####################################

.. meta::
   :description: Use Amazon CloudWatch code examples to write your Go applications.
   :keywords: AWS SDK for Go code examples, CloudWatch


|CWlong| is a web service that monitors your AWS resources and the applications
you run on AWS in real time. You can use |CW| to collect and track metrics, which are variables you
can measure for your resources and applications. |CW| alarms send notifications or automatically make
changes to the resources you are monitoring based on rules that you define.

The |sdk-go| examples show you how to integrate |CW| into your Go applications.
The examples assume you have already set up and configured the SDK (that
is, you have imported all required packages and set your credentials and region). For more information,
see :doc:`setting-up` and :doc:`configuring-sdk`.

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <cloudwatch>` repository on GitHub.

.. toctree::
   :titlesonly:
   :maxdepth: 1

   cw-example-describing-alarms
   cw-example-using-alarm-actions
   cw-example-getting-metrics
   cw-example-sending-events
   cw-example-getting-log-events
