.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _lambda-go-example-run-function:

########################
Running a |LAM| Function
########################

The following example rus the |LAM| function :code:`MyGetitemsFunction` in the :code:`us-west-2` region.
This function returns a list of items from a database. The input JSON looks like:

.. code-block:: json

   {
      "SortBy": "name|time",
      "SortOrder": "ascending|descending",
      "Number": 50
   }

where:

* :code:`SortBy` is the criteria for sorting the results. Our examples uses :code:`time`,
  which means the returned items are sorted in the order in which they were added to the database.

* :code:`SortOrder` is the order of sorting. Our example uses :code:`descending`,
  which means the most-recent item is last in the list.

* :code:`Number` is the maximum number of items to retrieve (the default is 50).
  Our example uses :code:`10`, which means get the 10 most-recent items.

The output JSON looks like:

.. code-block:: json

   {
      "statusCode": 200|...,
      "body": {
         "result": "'success' or 'failure'",
         "error": "Error message if 'failure', '' otherwise"
         "data": [
            {
               "item": "item1"
            },
            ...,
            {
               "item": "itemN"
            }
         ]
      }
   }

where:

* :code:`statusCode` is an HTTP status code, :code:`200` means the call was successful.
* :code:`body` is the body of the returned JSON.
* :code:`result` is the result of the call, either :code:`success` or :code:`failure`.
* :code:`error` is an error message if :code:`result` is :code:`failure`, otherwise an empty string
* :code:`data` is the returned results if :code:`result` is :code:`success`, otherwise nil.
* :code:`item` is an item from the list of results.

The first step is to import the packages we use:

* :code:`encoding/json` loads the JSON module we use to marshall and unmarshall the request and response payloads.
* :code:`fmt` is for displaying information to the user.
* :code:`os` is so we can terminate the application when we've entountered an error.
* :code:`strconv` is for converting an integer into a string.
* :code:`aws` loads the AWS SDK for Go package we use to invoke the |lam| function.
* :code:`session` is for creating a session.
* :code:`lambda` is for creating a |lam| client to invoke our function.

.. literalinclude:: ./example_code/lambda/aws-go-sdk-lambda-example-run-function.go
   :lines: 17-26
   :dedent: 0
   :language: go

We then create the |lam| client we use to invoke the |lam| function.

.. literalinclude:: ./example_code/lambda/aws-go-sdk-lambda-example-run-function.go
   :lines: 64
   :dedent: 4
   :language: go

Next we create the request argument and call :code:`MyGetItemsFunction`.

.. literalinclude:: ./example_code/lambda/aws-go-sdk-lambda-example-run-function.go
   :lines: 66-76
   :dedent: 4
   :language: go

Finally we parse the response, and if are successful, we print out the items.

.. literalinclude:: ./example_code/lambda/aws-go-sdk-lambda-example-run-function.go
   :lines: 83-111
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/lambda/aws-go-sdk-lambda-example-run-function.go>`_,
which includes the structures for marshalling the JSON request and unmarshalling the JSON response, on GitHub.
