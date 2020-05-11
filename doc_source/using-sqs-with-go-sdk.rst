.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-sqs:

#################################
|SQS| Examples Using the |sdk-go|
#################################

.. meta::
   :description: Use Amazon SQS code examples to write your own Go applications.
   :keywords: AWS SDK for Go code examples, SQS

|SQSlong| (|SQS|) is a fully managed message queuing service that makes it easy to decouple and scale microservices,
distributed systems, and serverless applications. The |sdk-go| examples can integrate |SQS| into your applications.
The examples assume you have already set up and configured the SDK (that
is, you've imported all required packages and set your credentials and region). For more information, see :doc:`setting-up` and :doc:`configuring-sdk`.

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <sqs>` repository on GitHub.

.. toctree::
   :titlesonly:
   :maxdepth: 1

   sqs-example-create-queue
   sqs-example-receive-message
   sqs-example-managing-visibility-timeout
   sqs-example-enable-long-polling
   sqs-example-dead-letter-queues
