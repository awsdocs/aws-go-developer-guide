.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-sqs-managing-visibility:

###########################################
Managing Visibility Timeout in |SQS| Queues
###########################################

.. meta::
   :description: Manage visibility timeout with Amazon SQS queues using this AWS SDK for Go code exammple.
   :keywords: AWS SDK for Go code examples, creating SQS queues


This |sdk-go| example shows you how to:

* Change visibility timeout with |SQS| queues

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <sqs>` repository on GitHub.

.. _sqs-visibility-scenario:

Scenario
========

This example manages visibility timeout with |SQS| queues. It uses these methods of the
|SQS| client class:

* :sdk-go-api-deep:`CreateQueue <service/sqs/#SQS.CreateQueue>`
* :sdk-go-api-deep:`ListQueues <service/sqs/#SQS.ListQueues>`
* :sdk-go-api-deep:`GetQueueUrl <service/sqs/#SQS.GetQueueUrl>`
* :sdk-go-api-deep:`DeleteQueue <service/sqs/#SQS.DeleteQueue>`


.. _sqs-visibility-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with using |SQS| visibility timeout. To learn more,
  see :sqs-dg:`Visibility Timeout <sqs-visibility-timeout>` in the |SQS-dg|.


.. _sqs-example-visibility-timeout:

Change the Visibility Timeout
=============================

Create a new Go file named :file:`sqs_changingvisibility.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/sqs/sqs_changingvisibility.go
   :lines: 15-23

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials.

.. literalinclude:: example_code/sqs/sqs_changingvisibility.go
   :lines: 27, 30-35

Get a message from the queue. Call ``ReceiveMessage``. Pass in the URL of the queue
to return details of the next message in the queue. Print any errors, or a
message if no message was received.

.. literalinclude:: example_code/sqs/sqs_changingvisibility.go
   :lines: 38-60

If a message was returned, use its receipt handle to set the timeout to 30 seconds.

.. literalinclude:: example_code/sqs/sqs_changingvisibility.go
   :lines: 63-76
