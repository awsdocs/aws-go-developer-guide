.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

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

Choose **Copy** to save the code locally.

Create the file :file:`events_schedule_rule.go`.
Import the packages used in the example.

.. literalinclude:: example_code/cloudwatch/events_schedule_rule.go
   :lines: 17-23
   :dedent: 0
   :language: go

Initialize a session that the SDK will use to load credentials
from the shared credentials file ~/.aws/credentials,
load your configuration from the shared configuration file ~/.aws/config,
and create a CloudWatch Events client.

.. literalinclude:: example_code/cloudwatch/events_schedule_rule.go
   :lines: 29-34
   :dedent: 4
   :language: go

Call ``PutRule``, supplying a name, :code:`DEMO_EVENT`,
ARN of the |IAM| role you created, :code:`IAM_ROLE_ARN`,
and an expression defining the schedule.
Finally, display the ARN of the rule.

.. literalinclude:: example_code/cloudwatch/events_schedule_rule.go
   :lines: 36-40, 46
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudwatch/events_schedule_rule.go>`_
on GitHub.

Add a Lambda Function Target
============================

Choose **Copy** to save the code locally.

Create the file :file:`events_put_targets.go`.
Import the packages used in the example.

.. literalinclude:: example_code/cloudwatch/events_put_targets.go
   :lines: 17-23
   :dedent: 0
   :language: go

Initialize a session that the SDK will use to load credentials
from the shared credentials file ~/.aws/credentials,
load your configuration from the shared configuration file ~/.aws/config,
and create a new CloudWatch Events client.
          
.. literalinclude:: example_code/cloudwatch/events_put_targets.go
   :lines: 29-34
   :dedent: 4
   :language: go

Call ``PutTargets``, supplying a name for the rule, :code:`DEMO_EVENT`.
For the target, specify the ARN of the Lambda function you created,
:code:`LAMBDA_FUNCTION_ARN`,
and the ID of the rule, :code:`myCloudWatchEventsTarget`.
Print any errors, or a success message with any targets that failed.

.. literalinclude:: example_code/cloudwatch/events_put_targets.go
   :lines: 36-44, 50
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudwatch/events_put_targets.go>`_
on GitHub.

Send Events
===========

Choose **Copy** to save the code locally.

Create the file :file:`events_put_events.go`.
Import the packages used in the example.

.. literalinclude:: example_code/cloudwatch/events_put_events.go
   :lines: 17-23
   :dedent: 0
   :language: go

Initialize a session that the SDK will use to load credentials
from the shared credentials file ~/.aws/credentials,
load your configuration from the shared configuration file ~/.aws/config,
and create a CloudWatch Events client.

.. literalinclude:: example_code/cloudwatch/events_put_events.go
   :lines: 29-34
   :dedent: 4
   :language: go

Call :code:`PutEvents`, supplying key-name value pairs in the :code:`Details` field,
and specifying the ARN of the Lambda function you created, :code:`RESOURCE_ARN`.
See :sdk-go-api-deep:`PutEventsRequestEntry <service/cloudwatchevents/#PutEventsRequestEntry>`
for a description of the fields.
Finally, display the ingested events.

.. literalinclude:: example_code/cloudwatch/events_put_events.go
   :lines: 36-47, 53
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudwatch/events_put_events.go>`_
on GitHub.

