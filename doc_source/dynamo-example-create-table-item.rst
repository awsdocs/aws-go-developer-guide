.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-dynamo-example-create-table-item:

###################################################
Creating an |DDBlong| Table Item Using the |sdk-go|
###################################################

.. meta::
   :description: Create a DynamoDB table item using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, DynamoDB

The following example uses the |DDB|
:sdk-go-api-deep:`PutItem <service/dynamodb/#DynamoDB.PutItem>` operation
to create the table item with the :code:`year` **2015** and
:code:`title`  **The Big New Movie** in the :code:`Movies` table in the
:code:`us-west-2` region.

Create the file *dynamodb_create_item.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/dynamodb/create_item.go
   :lines: 17-24
   :dedent: 0
   :language: go

Create the data structures we use to containing the information about the
table item.

.. literalinclude:: example_code/dynamodb/create_item.go
   :lines: 28-37
   :dedent: 0
   :language: go

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials,
and create a new |DDB| service client.

.. literalinclude:: example_code/dynamodb/create_item.go
   :lines: 42-47
   :dedent: 4
   :language: go

Initialize the structs with the movie data and marshall that data into a
map of **AttributeValue** objects.

.. literalinclude:: example_code/dynamodb/create_item.go
   :lines: 49-60
   :dedent: 4
   :language: go

Create the input for **PutItem** and call it.
If an error occurs, print the error and exit.
If no error occurs, print an message that the item was added to the table.

.. literalinclude:: example_code/dynamodb/create_item.go
   :lines: 69-82
   :dedent: 4
   :language: go

See the `complete example <https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/dynamodb/create_item.go>`_
on GitHub.
