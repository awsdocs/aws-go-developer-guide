.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.


#########################
|sdk-go| Request Examples
#########################

.. meta::
   :description: Learn how to work with requests in Go applications using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, requests



The |sdk-go| examples can help you write your own applications.
The examples assume you have already set up and configured the SDK (that
is, you have imported all required packages and set your credentials and region). For more
information, see :doc:`setting-up` and :doc:`configuring-sdk`.


.. _using-context-context-with-requests:

Using context.Context with SDK Requests
=======================================

In Go 1.7, the ``context.Context`` type was added to ``http.Request``. This type provides an easy
way to implement deadlines and cancellations on requests.

To use this pattern with the SDK, call ``WithContext`` on the ``HTTPRequest`` field
of the SDK's ``request.Request`` type, and provide your ``Context value``. The following example
highlights this process with a timeout on |SQS| ``ReceiveMessage``.

.. literalinclude:: extending.go.receive_message_request.txt
   :dedent: 4
   :language: go

.. _using-api-field-setters:

Using API Field Setters with SDK Requests
=========================================

In addition to setting API parameters by using struct fields, you can also use chainable
setters on the API operation parameter fields. This enables you to use a chain of setters
to set the fields of the API struct.

.. literalinclude:: s3.go.put_object.call.txt
   :dedent: 4
   :language: go

You can also use this pattern with nested fields in API operation requests.

.. literalinclude:: s3.go.update_deployment.call.txt
   :dedent: 4
   :language: go
