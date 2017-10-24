
.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-cw-sending-events:

###########################
Sending Events to |CWElong|
###########################

.. meta::
   :description: Work with the CloudWatch Events service using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, CloudWatch Events, targets, rules


These Go examples show you how to use the |sdk-go| to:

* Create and update a rule used to trigger an event
* Define one or more targets to respond to an event
* Send events that are matched to targets for handling

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <cloudwatch>` repository on GitHub.

.. _cw-get-sending-events-scenario:

Scenario
========

|CWE| delivers a near real-time stream of system events that describe changes in AWS resources
to any of various targets. Using simple rules, you can match events and route them to one or more target
functions or streams.

In these examples, Go code is used to send events to |CWE|. The code uses the |sdk-go| to manage instances
by using these methods of the :sdk-go-api-deep:`CloudWatchEvents <service/cloudwatchevents/#CloudWatchEvents>`
type:

* :sdk-go-api-deep:`PutRule <service/cloudwatchevents/#CloudWatchEvents.PutRule>`
* :sdk-go-api-deep:`PutTargets <service/cloudwatchevents/#CloudWatchEvents.PutTargets>`
* :sdk-go-api-deep:`PutEvents <service/cloudwatchevents/#CloudWatchEvents.PutEvents>`

.. _cw-sending-events-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with |CWE|. To learn more, see :cwe-ug:`Adding Events with PutEvents <AddEventsPutEvents>`
  in the |CWE-ug|.

Tasks Before You Start
======================

To set up and run this example, you must first complete these tasks:

1. Create a Lambda function using the hello-world blueprint to serve as the target for events.
   To learn how, see :cwe-dg:`Step 1: Create an AWS Lambda function <LogEC2InstanceState>` in the
   |CW| Events User Guide.

2. Create an |IAM| role whose policy grants permission to |CWE| and that includes
   ``events.amazonaws.com`` as a trusted entity. For more information about creating an |IAM| role, see
   :iam-ug:`Creating a Role to Delegate Permissions to an AWS Service <id_roles_create_for-service>`
   in the |iam-ug|.

   Use the following role policy when creating the |IAM| role.

.. code-block:: json

   {
      "Version": "2012-10-17",
      "Statement": [
         {
            "Sid": "CloudWatchEventsFullAccess",
            "Effect": "Allow",
            "Action": "events:*",
            "Resource": "*"
         },
         {
            "Sid": "IAMPassRoleForCloudWatchEvents",
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "arn:aws:iam::*:role/AWS_Events_Invoke_Targets"
         }
      ]
   }

Use the following trust relationship when creating the |IAM| role.

.. code-block:: json

   {
      "Version": "2012-10-17",
      "Statement": [
         {
            "Effect": "Allow",
            "Principal": {
               "Service": "events.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
         }
      ]
   }

.. _cw-example-scheduled-rule:

Create a Scheduled Rule
=======================

Create a new Go file named :file:`events_schedule_rule.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/cloudwatch/events_schedule_rule.go
   :lines: 15-23

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials, and create a new |EC2| service client.

.. literalinclude:: example_code/cloudwatch/events_put_events.go
   :lines: 27-34

Call ``PutRule``, supplying a name, ARN of the |IAM| role you created, and an expression defining the schedule.
Print any errors, or a success message.

.. literalinclude:: example_code/cloudwatch/events_put_events.go
   :lines: 36-48

Add a Lambda Function Target
============================

Create a new Go file named :file:`events_put_targets.go`.

Call the ``PutRule`` method to create the rule. The method returns the ARN of the new or updated rule.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/cloudwatch/events_put_targets.go
   :lines: 15-23

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials, and create a new |EC2| service client.

.. literalinclude:: example_code/cloudwatch/events_put_targets.go
   :lines: 27-34


Call ``PutTargets``, supplying a name for the rule. For the target, specify the ARN of the Lambda function
you created, and the ID of the rule. Print any errors, or a success message.

.. literalinclude:: example_code/cloudwatch/events_put_targets.go
   :lines: 36-52

Send Events
===========

Create a new Go file named :file:`events_put_events.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/cloudwatch/events_put_events.go
   :lines: 15-23

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials, and create a new |EC2| service client.

.. literalinclude:: example_code/cloudwatch/events_put_events.go
   :lines: 27-34


Call ``PutEvents``, supplying key-name value pairs in the Details field, and specifying the ARN of the
Lambda function you created. See :sdk-go-api-deep:`PutEventsRequestEntry <service/cloudwatchevents/#PutEventsRequestEntry>`
for a description of the fields. Print out any errors, or a success message.

.. literalinclude:: example_code/cloudwatch/events_put_events.go
   :lines: 36-55

