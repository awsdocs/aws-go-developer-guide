.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _lambda-go-example-configure-function-for-notification:

#####################################################
Configuring a |LAM| Function to Receive Notifications
#####################################################

.. meta::
    :description:
        Configure AWS Lambda functions using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, Lambda

The following example configures the |LAM| function :code:`functionName`
to accept notifications from the resource with the ARN :code:`sourceArn`.

The first step is to create the session and |LAM| client.

.. literalinclude:: ./example_code/lambda/aws-go-sdk-lambda-example-configure-function-for-notification.go
   :lines: 14-20
   :dedent: 0
   :language: go

Next, we create the structure for the input argument to the :code:`AddPermission` function.

.. literalinclude:: ./example_code/lambda/aws-go-sdk-lambda-example-configure-function-for-notification.go
   :lines: 22-28
   :dedent: 0
   :language: go

Finally, we call :code:`AddPermission` and display a message with the result of the call.

.. literalinclude:: ./example_code/lambda/aws-go-sdk-lambda-example-configure-function-for-notification.go
   :lines: 30-37
   :dedent: 0
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/lambda/aws-go-sdk-lambda-example-configure-function-for-notification.go>`_ on GitHub.
