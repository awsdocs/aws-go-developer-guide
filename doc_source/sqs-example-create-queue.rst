.. Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved. 
   SPDX-License-Identifier: CC-BY-SA-4.0

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

Create a new Go file named :file:`ListQueues.go`. You must import the relevant Go and
|sdk-go| packages by adding the following lines.

.. literalinclude:: sqs.go.list_queues.imports.txt
   :language: go
   :dedent: 0

Initialize a session that the SDK uses to load credentials from the shared credentials file, ~/.aws/credentials
and the default region from ~/.aws/config.

.. literalinclude:: sqs.go.list_queues.sess.txt
   :language: go
   :dedent: 4

Create a service client and call ``ListQueues``,
passing in ``nil`` to return all queues. 

.. literalinclude:: sqs.go.list_queues.call.txt
   :language: go
   :dedent: 4

Loop through the queue URLs to print them.

.. literalinclude:: sqs.go.list_queues.display.txt
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/ListQueues/ListQueues.go>`_
on GitHub.
            
Create a Queue
==============

Create a new Go file named :file:`CreateQueue.go`. You must import the relevant Go and
|sdk-go| packages by adding the following lines.

.. literalinclude:: sqs.go.create_queue.imports.txt
   :language: go
   :dedent: 0

Get the queue name from the command line.

.. literalinclude:: sqs.go.create_queue.args.txt
   :language: go
   :dedent: 4

Initialize a session that the SDK uses to load credentials from the shared credentials file, ~/.aws/credentials
and the default region from ~/.aws/config.

.. literalinclude:: sqs.go.create_queue.sess.txt
   :language: go
   :dedent: 4

Create a service client and call ``CreateQueue``,
passing in the queue name and queue attributes. 

.. literalinclude:: sqs.go.create_queue.call.txt
   :language: go
   :dedent: 4

Display the queue URL.

.. literalinclude:: sqs.go.create_queue.print.txt
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/CreateQueue/CreateQueue.go>`_
on GitHub.

Get the URL of a Queue
======================

Create a new Go file named :file:`GetQueueUrl.go`. You must import the relevant Go and
|sdk-go| packages by adding the following lines.

.. literalinclude:: sqs.go.get_queue_url.imports.txt
   :language: go
   :dedent: 0

Get the name of the queue from the command line.

.. literalinclude:: sqs.go.get_queue_url.args.txt
   :language: go
   :dedent: 4

Initialize a session that the SDK uses to load credentials from the shared credentials file, ~/.aws/credentials
and the default region from ~/.aws/config.

.. literalinclude:: sqs.go.get_queue_url.sess.txt
   :language: go
   :dedent: 4

Create a service client and call ``GetQueueUrl``,
passing in the queue name.

.. literalinclude:: sqs.go.get_queue_url.call.txt
   :language: go
   :dedent: 4

Display the URL of the queue.

.. literalinclude:: sqs.go.get_queue_url.print.txt
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/GetQueueURL/GetQueueURL.go>`_
on GitHub.

Delete a Queue
==============

Create a new Go file named :file:`DeleteQueue.go`. You must import the relevant Go and
|sdk-go| packages by adding the following lines.

.. literalinclude:: sqs.go.delete_queue.imports.txt
   :language: go
   :dedent: 0

Get the name of the queue from the command line.

.. literalinclude:: sqs.go.delete_queue.args.txt
   :language: go
   :dedent: 4

Initialize a session that the SDK uses to load credentials from the shared credentials file, ~/.aws/credentials
and the default region from ~/.aws/config.

.. literalinclude:: sqs.go.delete_queue.sess.txt
   :language: go
   :dedent: 4

Create a service client and all ``DeleteQueue`` passing in the queue name.

.. literalinclude:: sqs.go.delete_queue.get_queue_url.txt
   :language: go
   :dedent: 4

See the  `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/sqs/DeleteQueue/DeleteQueue.go>`_
on GitHub.
