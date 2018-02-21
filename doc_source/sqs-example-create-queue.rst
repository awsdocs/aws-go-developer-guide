.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-sqs-using-queues:

##################
Using |SQS| Queues
##################

.. meta::
   :description: Learn to work with Amazon SQS queues using this AWS SDK for Go.
   :keywords: AWS SDK for Go code examples, SQS, create queue


These |sdk-go| examples show you how to:

*  List |SQS| queues
*  Create |SQS| queues
*  Get |SQS| queue URLs
*  Delete |SQS| queues

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <sqs>` repository on GitHub.

.. _sqs-create-scenario:

Scenario
========

These examples demonstrate how to work with |SQS| queues.

The code uses these methods of the |SQS| client class:

* :sdk-go-api-deep:`CreateQueue <service/sqs/#SQS.CreateQueue>`
* :sdk-go-api-deep:`ListQueues <service/sqs/#SQS.ListQueues>`
* :sdk-go-api-deep:`GetQueueUrl <service/sqs/#SQS.GetQueueUrl>`
* :sdk-go-api-deep:`DeleteQueue <service/sqs/#SQS.DeleteQueue>`

.. _sqs-create-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with using |SQS|. To learn more, see :sqs-dg:`How Queues Work <sqs-how-it-works>` in the |SQS-dg|.


.. _sqs-example-list-queues:

List Queues
===========

Create a new Go file named :file:`sqs_listqueues.go`. You must import the relevant Go and
|sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/sqs/sqs_listqueues.go
   :lines: 15-22

Initialize a session that the SDK will use to load credentials from the shared credentials file, ~/.aws/credentials.

.. literalinclude:: example_code/sqs/sqs_listqueues.go
   :lines: 26,29-34

Call ``ListQueues`` passing in ``nil`` to return all queues. Print any errors or a success
message and loop through the queue URLs to print them.

.. literalinclude:: example_code/sqs/sqs_listqueues.go
   :lines: 37-52

Create Queues
=============

Create a new Go file named :file:`sqs_createqueues.go`. You must import the relevant Go and
|sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/sqs/sqs_createqueues.go
   :lines: 15-23

Initialize a session that the SDK will use to load credentials from the shared credentials file, ~/.aws/credentials.

.. literalinclude:: example_code/sqs/sqs_createqueues.go
   :lines: 27,30-35

Call ``CreateQueue`` passing in the new queue name and queue attributes. Print any errors
or a success message.

.. literalinclude:: example_code/sqs/sqs_createqueues.go
   :lines: 37-50


Get a Queue URL
===============

Create a new Go file named :file:`sqs_getqueueurl.go`. You must import the relevant Go and
|sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/sqs/sqs_getqueueurl.go
   :lines: 15-23

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials.

.. literalinclude:: example_code/sqs/sqs_getqueueurl.go
   :lines: 27,30-35

Call ``GetQueueUrl`` passing in the queue name. Print any errors
or a success message.

.. literalinclude:: example_code/sqs/sqs_getqueueurl.go
   :lines: 37-47

Delete a Queue
==============

Create a new Go file named :file:`sqs_deletequeue.go`. You must import the relevant Go and
|sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/sqs/sqs_deletequeue.go
   :lines: 15-23

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials.

.. literalinclude:: example_code/sqs/sqs_deletequeue.go
   :lines: 27,30-35

Call ``DeleteQueue`` passing in the queue name. Print any errors
or a success message.

.. literalinclude:: example_code/sqs/sqs_deletequeue.go
   :lines: 37-47

