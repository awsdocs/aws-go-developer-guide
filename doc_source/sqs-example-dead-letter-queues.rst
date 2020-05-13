.. Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved. 
   SPDX-License-Identifier: CC-BY-SA-4.0

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

Create a new Go file with the name :file:`DeadLetterQueue.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: sqs.go.dead_letter_queue.imports.txt
   :language: go
   :dedent: 0

Get the name of the queue and the dead letter queue from the commmand line.

.. literalinclude:: sqs.go.dead_letter_queue.args.txt
   :language: go
   :dedent: 0

Initialize a session that the SDK will use to load credentials
from the shared credentials file, *~/.aws/credentials*
and the default AWS Region from *~/.aws/config*.

.. literalinclude:: sqs.go.dead_letter_queue.sess.txt
   :language: go,
   :dedent: 4

Create a service client and call ``GetQueueUrl`` to get the URL for the queue.

.. literalinclude:: sqs.go.get_queue_url.call.txt
   :language: go
   :dedent: 4

The URL of the queue is in the ``QueueUrl`` property of the returned object.

.. literalinclude:: sqs.go.dead_letter_queue.url.txt
   :language: go
   :dedent: 4

Similarly, get the URL of the dead letter queue.

Create the ARN of the dead-letter queue from the URL.

.. literalinclude:: sqs.go.get_queue_url.arn.txt
   :language: go
   :dedent: 4

Define the redrive policy for the queue.

.. literalinclude:: sqs.go.dead_letter_queue.policy.txt
   :language: go
   :dedent: 4

Marshal the policy to use as input
for the ``SetQueueAttributes`` call.           

.. literalinclude:: sqs.go.dead_letter_queue.marshall.txt
   :language: go
   :dedent: 4

Set the policy on the queue.

.. literalinclude:: sqs.go.dead_letter_queue.set_attributes.txt
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/DeadLetterQueue/DeadLetterQueue.go>`_
on GitHub.
