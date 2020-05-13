.. Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved. 
   SPDX-License-Identifier: CC-BY-SA-4.0

.. _examples-sqs-receive-message:

#######################################
Sending and Receiving Messages in |SQS|
#######################################

.. meta::
   :description: Send or receive a message from an Amazon SQS queue using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go examples, SQS queues

These |sdk-go| examples show you how to:

* Send a message to an |SQS| queue
* Receive a message from an |SQS| queue
* Delete a message from an |SQS| queue

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <sqs>` repository on GitHub.

.. _sqs-receive-message-scenario:

The Scenario
============

These examples demonstrate sending, receiving, and deleting messages from an |SQS| queue.

The code uses these methods of the |SQS| client class:

* :sdk-go-api-deep:`SendMessage <service/sqs/#SQS.SendMessage>`
* :sdk-go-api-deep:`ReceiveMessage <service/sqs/#SQS.ReceiveMessage>`
* :sdk-go-api-deep:`DeleteMessage <service/sqs/#SQS.DeleteMessage>`
* :sdk-go-api-deep:`GetQueueUrl <service/sqs/#SQS.GetQueueUrl>`

.. _sqs-receive-message-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with the details of |SQS| messages. To learn more, see
  :sqs-dg:`Sending a Message to an Amazon SQS Queue <sqs-send-message>` and
  :sqs-dg:`Receiving and Deleting a Message from an Amazon SQS Queue <sqs-receive-delete-message>`
  in the |SQS-dg|.

.. _sqs-example-send-message:

Send a Message to a Queue
=========================

Create a new Go file named :file:`SendMessage.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: sqs.go.send_message.imports.txt
   :language: go
   :dedent: 0

Get the name of the queue from the command line.

.. literalinclude:: sqs.go.send_message.args.txt
   :language: go
   :dedent: 4

Initialize a session that the SDK uses to load credentials
from the shared credentials file, ~/.aws/credentials
and the default region from ~/.aws/config.

.. literalinclude:: sqs.go.send_message.sess.txt
   :language: go
   :dedent: 4

Create a service client and call `SendMessage`.
The input represents information about a fiction best seller for a particular week and defines title,
author, and weeks on the list values.

.. literalinclude:: sqs.go.send_message.call.txt
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/SendMessage/SendMessage.go>`_
on GitHub.

.. _sqs-example-receive-mesage:

Receiving a Message from a Queue
================================

Create a new Go file named :file:`ReceiveMessage.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: sqs.go.receive_messages.imports.txt
   :language: go
   :dedent: 0

Get the name of the queue and how long the message is hidden from other consumers from the command line.

.. literalinclude:: sqs.go.receive_messages.args.txt
   :language: go
   :dedent: 4

Initialize a session that the SDK uses to load credentials
from the shared credentials file, ~/.aws/credentials
and the default region from ~/.aws/config.

.. literalinclude:: sqs.go.receive_messages.sess.txt
   :language: go
   :dedent: 4

Create a service client and call the `GetQueueUrl` function
to get the URL of the queue.

.. literalinclude:: sqs.go.receive_messages.queue_url.txt
   :language: go
   :dedent: 4

The URL of the queue is in the `QueueUrl` property of the returned object.

.. literalinclude:: sqs.go.receive_message.url.txt
   :language: go
   :dedent: 4

Call the `ReceiveMessage` function to get the messages in the queue.

.. literalinclude:: sqs.go.receive_messages.call.txt
   :language: go
   :dedent: 4

Print the message handle of the first message in the queue
(you need the handle to delete the message).

.. literalinclude:: sqs.go.receive_messages.print_handle.txt
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/ReceiveMessage/ReceiveMessage.go>`_
on GitHub.

.. _sqs-example-delete-message:

Delete a Message from a Queue
=============================

Create a new Go file named :file:`DeleteMessage.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: sqs.go.delete_message.imports.txt
   :language: go
   :dedent: 0

Get the name of the queue and the handle for the message from the command line.

.. literalinclude:: sqs.go.delete_message.args.txt
   :language: go
   :dedent: 4

Initialize a session that the SDK uses to load credentials
from the shared credentials file, ~/.aws/credentials
and the default region from ~/.aws/config.

.. literalinclude:: sqs.go.delete_message.sess.txt
   :language: go
   :dedent: 4

Create a service client and call the `DeleteMessage` function,
passing in the name of the queue and the message handle.

.. literalinclude:: sqs.go.delete_message.call.txt
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/DeleteMessage/DeleteMessage.go>`_
on GitHub.
