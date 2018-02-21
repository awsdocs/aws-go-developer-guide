.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-sqs-receive-message:

#######################################
Sending and Receiving Messages in |SQS|
#######################################

.. meta::
   :description: Send or receive a message from an Amazon SQS queue using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go examples, SQS queues

These |sdk-go| examples show you how to:

* Send a message to an |SQS| queue
* Receive and delete a message from an |SQS| queue
* Send and receive messages from an |SQS| queue

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

Create a new Go file named :file:`sqs_sendmessage.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/sqs/sqs_sendmessage.go
   :lines: 15-23

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials.

.. literalinclude:: example_code/sqs/sqs_sendmessage.go
   :lines: 27-35

Now you're ready to send your message. In the example, the message input passed to ``SendMessage``
represents information about a fiction best seller for a particular week and defines title,
author, and weeks on the list values.

.. literalinclude:: example_code/sqs/sqs_sendmessage.go
   :lines: 37-63

.. _sqs-example-receive-delete-message:

Receive and Delete a Message from a Queue
=========================================

Create a new Go file named :file:`sqs_deletemessage.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/sqs/sqs_deletemessage.go
   :lines: 15-23

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials.

.. literalinclude:: example_code/sqs/sqs_deletemessage.go
   :lines: 27-32

Now you're ready to receive a message from a queue specified by a queue URL. In the example,
the ``qURL`` variable would hold the URL for the queue containing the message.

.. literalinclude:: example_code/sqs/sqs_deletemessage.go
   :lines: 35-58

After retrieving the message, delete it from the queue with ``DeleteMessage``, passing the ``ReceiptHandle``
returned from the previous call.

.. literalinclude:: example_code/sqs/sqs_deletemessage.go
   :lines: 60-71

.. _sqs-example-send-receive-message:

Send and Receive Messages
=========================

Create a new Go file named :file:`sqs_longpolling_receive_message.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/sqs/sqs_longpolling_receive_message.go
   :lines: 15-26

Get the queue name and timeout passed from the command.

.. literalinclude:: example_code/sqs/sqs_longpolling_receive_message.go
   :lines: 32-42

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials.

.. literalinclude:: example_code/sqs/sqs_longpolling_receive_message.go
   :lines: 46-51

Get the Queue. You need to convert the queue name into a URL. You can use the ``GetQueueUrl``
API call to retrieve the URL. This is needed for receiving messages from the queue. Print
any errors.

.. literalinclude:: example_code/sqs/sqs_longpolling_receive_message.go
   :lines: 56-64

Call ``ReceiveMessage`` to get the latest message from the queue.

.. literalinclude:: example_code/sqs/sqs_longpolling_receive_message.go
   :lines: 67-86

The example uses this utility function.

.. literalinclude:: example_code/sqs/sqs_longpolling_receive_message.go
   :lines: 88-91

