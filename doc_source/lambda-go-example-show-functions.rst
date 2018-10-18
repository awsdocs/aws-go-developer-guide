.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _lambda-go-example-show-functions:

################################################
Displaying Information about All |LAM| Functions
################################################

.. meta::
    :description:
        Display information about AWS Lambda functions using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, Lambda

First import the packages we use in this example.

.. literalinclude:: ./example_code/lambda/aws-go-sdk-lambda-example-show-functions.go
   :lines: 17-24
   :dedent: 0
   :language: go

Next, create the session and |LAM| client.

.. literalinclude:: ./example_code/lambda/aws-go-sdk-lambda-example-show-functions.go
   :lines: 29-32, 34
   :dedent: 4
   :language: go

Next, call :code:`ListFunctions` and exit if there is an error.

.. literalinclude:: ./example_code/lambda/aws-go-sdk-lambda-example-show-functions.go
   :lines: 36-40
   :dedent: 4
   :language: go
	       
Finally, display the names and descriptions of the
|LAM| functions.

.. literalinclude:: ./example_code/lambda/aws-go-sdk-lambda-example-show-functions.go
   :lines: 42-48
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/lambda/aws-go-sdk-lambda-example-show-functions.go>`_ on GitHub.
