.. Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved. 
   SPDX-License-Identifier: CC-BY-SA-4.0

.. _examples-sqs-long-polling:

#####################################
Enabling Long Polling in |SQS| Queues
#####################################

.. meta::
   :description: Enable long polling with Amazon SQS queues using this AWS SDK code example.
   :keywords: AWS SDK for Go code examples, SQS, create queue

These |sdk-go| examples show you how to:

* Enable long polling when you create an |SQS| queue
* Enable long polling on an existing |SQS| queue
* Enable long polling when a message is received

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <sqs>` repository on GitHub.

.. _sqs-long-polling-scenario:

Scenario
========

Long polling reduces the number of empty responses by allowing |SQS|
to wait a specified time for a message to become available in the queue before sending a response.
Also, long polling eliminates false empty responses by querying all of the servers instead of a
sampling of servers. To enable long polling, you must specify a non-zero wait time for received
messages. You can do this by setting the ``ReceiveMessageWaitTimeSeconds`` parameter of a queue
or by
setting the ``WaitTimeSeconds`` parameter on a message when it is received.

The code uses these methods of the |SQS| client class:

* :sdk-go-api-deep:`CreateQueue <service/sqs/#SQS.CreateQueue>`

* :sdk-go-api-deep:`SetQueueAttributes <service/sqs/#SQS.SetQueueAttributes>`
* :sdk-go-api-deep:`ReceiveMessage <service/sqs/#SQS.ReceiveMessage>`


.. _sqs-long-polling-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with |SQS| polling. To learn more, see
  :sqs-dg:`Long Polling <sqs-long-polling>` in the |SQS-dg|.

.. _sqs-example-create-queue-long-pollling:

Enable Long Polling When Creating a Queue
=========================================

This example creates a queue with long polling enabled. If the queue already exists,
no error is returned.

Create a new Go file named :file:`CreateLPQueue.go`. You must import the
relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: sqs.go.create_lp_queue.imports.txt
   :language: go
   :dedent: 0

Get the queue name and wait time from the command line.
Ensure that the wait time is between 0 (zero) and 20 seconds.

.. literalinclude:: sqs.go.create_lp_queue.args.txt
   :language: go
   :dedent: 4

Initialize a session that the SDK will use to load credentials
from the shared credentials file, *~/.aws/credentials*
and the default AWS Region from *~/.aws/config*.

.. literalinclude:: sqs.go.create_lp_queue.sess.txt
   :language: go
   :dedent: 4

Create a service client and call ``CreateQueue``,
passing in the time to wait for messages.

.. literalinclude:: sqs.go.create_lp_queue.call.txt
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/CreateLPQueue/CreateLPQueue.go>`_
on GitHub.

Enable Long Polling on an Existing Queue
========================================

Create a new Go file named :file:`ConfigureLPQueue.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: sqs.go.configure_lp_queue.imports.txt
   :language: go
   :dedent: 0

Get the queue name and the optional timeout value from the command line.
Ensure that the wait time is between 1 and 20 seconds.

.. literalinclude:: sqs.go.configure_lp_queue.args.txt
   :language: go
   :dedent: 4

Initialize a session that the SDK will use to load credentials
from the shared credentials file, *~/.aws/credentials*,
and a default AWS Region from *~/.aws/config*.

.. literalinclude:: sqs.go.configure_lp_queue.sess.txt
   :language: go
   :dedent: 4

Get the URL of the queue.

.. literalinclude:: sqs.go.configure_lp_queue.get_url.txt
   :language: go
   :dedent: 4

The URL is in the QueueUrl property of the returned object.

.. literalinclude:: sqs.go.configure_lp_queue.url.txt
   :language: go
   :dedent: 4

Update the queue to enable long polling with a call to ``SetQueueAttributes``, passing in the
queue URL.

.. literalinclude:: sqs.go.configure_lp_queue.set_attributes.txt
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/ConfigureLPQueue/ConfigureLPQueue.go>`_
on GitHub.

Enable Long Polling on Message Receipt
======================================

Create a new Go file named :file:`ReceiveLPMessage.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: sqs.go.receive_lp_message.imports.txt
   :language: go
   :dedent: 0

Get the queue name and optional visibility and wait time values from the command line.
Ensure that the visibility is between 0 (zero) seconds and 12 hours
and that the wait time is between 0 and 20 seconds.

.. literalinclude:: sqs.go.receive_lp_message.args.txt
   :language: go
   :dedent: 4

Initialize a session that the SDK will use to load credentials
from the shared credentials file, *~/.aws/credentials*
and the default AWS Region from *~/.aws/config*.

.. literalinclude:: sqs.go.receive_lp_message.sess.txt
   :language: go
   :dedent: 4

Create a service client and call ``GetQueueUrl`` to get the URL of the queue.

.. literalinclude:: sqs.go.get_queue_url.call.txt
   :language: go
   :dedent: 4

Call ``ReceiveMessage`` to get the messages, using long polling, from the queue.

.. literalinclude:: sqs.go.receive_lp_message.call.txt
   :language: go
   :dedent: 4

Display the IDs of the mesages.

.. literalinclude:: sqs.go.receive_lp_message.display.txt
   :language: go
   :dedent: 0

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/ReceiveLPMessage/ReceiveLPMessage.go>`_
on GitHub.
