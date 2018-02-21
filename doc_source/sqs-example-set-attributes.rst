.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-sqs-set-attributes:

####################################
Setting Attributes on an |SQS| Queue
####################################

.. meta::
   :description: Set attributes on an Amazon SQS queue using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, SQS, create queue

This Go example shows you how to set attributes on an existing |SQS| queue.

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <sqs>` repository on GitHub.

.. _sqs-attributes-scenario:

Scenario
========

This example updates an existing |SQS| queue to use long polling.

Long polling reduces the number of empty responses by allowing |SQS|
to wait a specified time for a message to become available in the queue before sending a response.
Also, long polling eliminates false empty responses by querying all of the servers instead of a
sampling of servers. To enable long polling, you must specify a non-zero wait time for received
messages. You can do this by setting the `ReceiveMessageWaitTimeSeconds` parameter of a queue or by
setting the `WaitTimeSeconds` parameter on a message when it is received.

The code uses these methods of the |SQS| client class:

* :sdk-go-api-deep:`GetQueueUrl <service/sqs/#SQS.GetQueueUrl>`
* :sdk-go-api-deep:`SetQueueAttributes <service/sqs/#SQS.SetQueueAttributes>`

If you are unfamiliar with using |SQS| long polling, you should read
:sqs-dg:`Long Polling <sqs-long-polling>` in the |SQS-dg| before proceeding.

.. _sqs-attributes-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with using |SQS| long polling. To learn more,
  see :sqs-dg:`Long Polling <sqs-long-polling>` in the |SQS-dg|.


.. _sqs3-example-set-attrs:

Set Attributes on Queue
=======================

Create a new Go file named :file:`sqs_longpolling_existing_queue.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/sqs/sqs_longpolling_existing_queue.go
   :lines: 15-27

Get the queue name and timeout passed in by the user.

.. literalinclude:: example_code/sqs/sqs_longpolling_existing_queue.go
   :lines: 33-43

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials.

.. literalinclude:: example_code/sqs/sqs_longpolling_existing_queue.go
   :lines: 47-52

Get the queue. You need to convert the queue name into a URL. You can use the ``GetQueueUrl``
API call to retrieve the URL. This is needed for setting attributes on the queue. Print
any errors.

.. literalinclude:: example_code/sqs/sqs_longpolling_existing_queue.go
   :lines: 57-65

Update the queue to enable long polling.

.. literalinclude:: example_code/sqs/sqs_longpolling_existing_queue.go
   :lines: 68-79

The example uses this utility function.

.. literalinclude:: example_code/sqs/sqs_longpolling_existing_queue.go
   :lines: 81-84

