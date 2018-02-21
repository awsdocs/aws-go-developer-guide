.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-sqs-using-dead-letter-queues:

#################################
Using Dead Letter Queues in |SQS|
#################################

.. meta::
   :description: Use an Amazon SQS queue to receive and hold messages with this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, SQS, create queue


This |sdk-go| example shows you how to configure source |SQS| queues that send messages to
a dead letter queue.

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <sqs>` repository on GitHub.

.. _sqs-dead-letter-queue-scenario:

Scenario
========

A dead letter queue is one that other (source) queues can target for messages that can't be
processed successfully. You can set aside and isolate these messages in the dead letter queue
to determine why their processing didn't succeed. You must individually configure each source
queue that sends messages to a dead letter queue. Multiple queues can target a single dead
letter queue.

The code uses this method of the |SQS| client class:

* :sdk-go-api-deep:`SetQueueAttributes <service/sqs/#SQS.SetQueueAttributes>`

.. _sqs-dead-letter-queue-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with |SQS| dead letter queues. To learn more,
  see :sqs-dg:`Using Amazon SQS Dead Letter Queues <sqs-dead-letter-queues>` in the |SQS-dg|.

.. _sqs-example-configure-source-queues:

Configure Source Queues
=======================

After you create a queue to act as a dead letter queue, you must configure the other queues
that route unprocessed messages to the dead letter queue. To do this, specify a redrive policy
that identifies the queue to use as a dead letter queue and the maximum number of receives by
individual messages before they are routed to the dead letter queue.

Create a new Go file with the name :file:`sqs_deadletterqueue.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/sqs/sqs_deadletterqueue.go
   :lines: 15-24

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials.

.. literalinclude:: example_code/sqs/sqs_deadletterqueue.go
   :lines: 28,31-36

Define the redrive policy for the queue, then marshal the policy to use as input
for the ``SetQueueAttributes`` call.

.. literalinclude:: example_code/sqs/sqs_deadletterqueue.go
   :lines: 39-43,46-50

Set the policy on the queue.

.. literalinclude:: example_code/sqs/sqs_deadletterqueue.go
   :lines: 52-65

