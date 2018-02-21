.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

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

* :sdk-go-api-deep:`SetQueueAttributes <service/sqs/#SQS.SetQueueAttributes>`
* :sdk-go-api-deep:`ReceiveMessage <service/sqs/#SQS.ReceiveMessage>`
* :sdk-go-api-deep:`CreateQueue <service/sqs/#SQS.CreateQueue>`

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

Create a new Go file named :file:`sqs_longpolling_create_queue.go`. You must import the
relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/sqs/sqs_longpolling_create_queue.go
   :lines: 15-26

Get the queue name passed in by the user.

.. literalinclude:: example_code/sqs/sqs_longpolling_create_queue.go
   :lines: 33-43

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials.

.. literalinclude:: example_code/sqs/sqs_longpolling_create_queue.go
   :lines: 47-52

Create the queue with long polling enabled. Print any errors or a success message.

.. literalinclude:: example_code/sqs/sqs_longpolling_create_queue.go
   :lines: 55-67

The example uses this utility function.

.. literalinclude:: example_code/sqs/sqs_longpolling_create_queue.go
   :lines: 69-72


Enable Long Polling on an Existing Queue
========================================

Create a new Go file named :file:`sqs_longpolling_existing_queue.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/sqs/sqs_longpolling_existing_queue.go
   :lines: 15-27

This example takes two flags, the -n flag is the queue name, and the -t flag contains the
timeout value.

.. literalinclude:: example_code/sqs/sqs_longpolling_existing_queue.go
   :lines: 33-43

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials.

.. literalinclude:: example_code/sqs/sqs_longpolling_existing_queue.go
   :lines: 47-52

You need to convert the queue name into a URL.
Make the ``GetQueueUrl`` API call to retrieve the URL. This is needed for setting attributes
on the queue.

.. literalinclude:: example_code/sqs/sqs_longpolling_existing_queue.go
   :lines: 57-65

Update the queue to enable long polling with a call to ``SetQueueAttributes``, passing in the
queue URL. Print any errors or a success message.

.. literalinclude:: example_code/sqs/sqs_longpolling_existing_queue.go
   :lines: 68-84

Enable Long Polling on Message Receipt
======================================

Create a new Go file named :file:`sqs_longpolling_receive_message.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/sqs/sqs_longpolling_receive_message.go
   :lines: 15-26

This example takes two flags, the -n flag is the queue name, and the -t flag contains the
timeout value.

.. literalinclude:: example_code/sqs/sqs_longpolling_receive_message.go
   :lines: 32-42

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials.

.. literalinclude:: example_code/sqs/sqs_longpolling_receive_message.go
   :lines: 46-51

You need to convert the queue name into a URL.
Make the ``GetQueueUrl`` API call to retrieve the URL. This is needed for setting attributes
on the queue.

.. literalinclude:: example_code/sqs/sqs_longpolling_receive_message.go
   :lines: 56-64

Receive a message from the queue with long polling enabled with a call to
``ReceiveMessage``, passing in the queue URL. Print any errors or a success message.

.. literalinclude:: example_code/sqs/sqs_longpolling_receive_message.go
   :lines: 67-91


