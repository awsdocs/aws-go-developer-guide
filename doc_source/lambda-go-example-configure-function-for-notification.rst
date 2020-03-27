.. Copyright 2010-2020 Amazon.com, Inc. or its affiliates. All Rights Reserved.

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

Import the packages we use.

.. literalinclude:: lambda.go.configure_function.imports.txt
   :dedent: 0
   :language: go

Get the name of the function and ARN of the entity invoking the function.
If either is missing,
print an error message and quit.

.. literalinclude:: lambda.go.configure_function.vars.txt
   :dedent: 4
   :language: go

Create the session and |LAM| client.

.. literalinclude:: lambda.go.configure_function.session.txt
   :dedent: 4
   :language: go

Create the structure for the input argument to the :code:`AddPermission` function.

.. literalinclude:: lambda.go.configure_function.struct.txt
   :dedent: 4
   :language: go

Finally, call :code:`AddPermission` and display a message with the result of the call.

.. literalinclude:: lambda.go.configure_function.add_permission.txt
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/lambda/aws-go-sdk-lambda-example-configure-function-for-notification.go>`_ on GitHub.
