.. Copyright 2010-2020 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _lambda-go-example-create-function:

#########################
Creating a |LAM| Function
#########################

.. meta::
    :description:
        Create AWS Lambda functions using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, Lambda

The following example creates the |LAM| function :code:`functionName`
in the :code:`us-west-2` region using the following values:

* Role ARN: :code:`resourceArn`.
  In most cases, you need to attach only the
  :code:`AWSLambdaExecute` managed policy
  to the policy for this role.

* Function entry point: :code:`handler`

* Runtime: :code:`runtime`

* Zip file: :code:`zipFileName` + :code:`.zip`

* Bucket: :code:`bucketName`

* Key: :code:`zipFileName`

The first step is to import the packages we use in the example.

.. literalinclude:: lambda.go.create_function.imports.txt
   :dedent: 0
   :language: go

Get the zip file name, bucket name, function name, handler, ARN,
and runtime values from the command line.
If any are missing, print and error message and quit.

.. literalinclude:: lambda.go.create_function.vars.txt
   :dedent: 4
   :language: go

Create the session and |LAM| client
and get the contents of the zip file.
If you cannot read the zip file contents,
print an error message and quit.

.. literalinclude:: lambda.go.create_function.session.txt
   :dedent: 4
   :language: go

Next, create the structures for the input argument to the :code:`CreateFunction` function.

.. literalinclude:: lambda.go.create_function.structs.txt
   :dedent: 4
   :language: go

Finally, call :code:`CreateFunction` and display a message with the result of the call.

.. literalinclude:: lambda.go.create_function.create.txt
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/lambda/aws-go-sdk-lambda-example-create-function.go>`_ on GitHub.
