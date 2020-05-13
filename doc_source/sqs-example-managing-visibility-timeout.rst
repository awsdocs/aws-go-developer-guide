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

This example manages visibility timeout with |SQS| queues.
Visibility is the duration, in seconds, while messages are in the queue, but not available to other consumers.
It uses these methods of the
|SQS| client class:

* :sdk-go-api-deep:`ChangeMessageVisibility <service/sqs/#SQS.ChangeMessageVisibility>`
* :sdk-go-api-deep:`GetQueueUrl <service/sqs/#SQS.GetQueueUrl>`

.. _sqs-visibility-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with using |SQS| visibility timeout. To learn more,
  see :sqs-dg:`Visibility Timeout <sqs-visibility-timeout>` in the |SQS-dg|.

.. _sqs-example-visibility-timeout:

Change the Visibility Timeout
=============================

Create a new Go file named :file:`ChangeMsgVisibility.go`.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: sqs.go.change_message_visibility.imports.txt
   :language: go
   :dedent: 0

Get the queue name, receipt handle of the message, and visibility duration from the command line.
Ensure the visibility is from 0 (zero) seconds to 12 hours.

.. literalinclude:: sqs.go.change_message_visibility.args.txt
   :language: go
   :dedent: 4

Initialize a session that the SDK will use to load credentials
from the shared credentials file, ~/.aws/credentials
and the default region from ~/.aws/config.

.. literalinclude:: sqs.go.change_message_visibility.sess.txt
   :language: go
   :dedent: 4

Create a service client and call ``GetQueueUrl`` to retrieve the URL of the queue.

.. literalinclude:: sqs.go.get_queue_url.call.txt
   :language: go
   :dedent: 4

The URL of the queue is in the ``QueueUrl`` property of the returned object.

.. literalinclude:: sqs.go.change_message_visibility.url.txt
   :language: go
   :dedent: 4

Call ``ChangeMessageVisibility`` to change the visibility of the messages in the queue.

.. literalinclude:: sqs.go.change_message_visibility.op.txt
   :language: go
   :dedent: 4

See the  `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/ChangeMsgVisibility/ChangeMsgVisibility.go>`_
on GitHub.
