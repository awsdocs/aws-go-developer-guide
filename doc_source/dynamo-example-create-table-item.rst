.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-dynamo-example-create-table-item:

################################
Creating an |DDBlong| Table Item
################################

.. meta::
   :description: Create a DynamoDB table item using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, DynamoDB

The following example uses the |DDB| |nbsp| 
:sdk-go-api-deep:`PutItem <service/dynamodb/#DynamoDB.PutItem>` operation
to create the table item with the :code:`year` **2015** and
:code:`title`  **The Big New Movie** in the :code:`Movies` table in 
your default region.

Create the file *DynamoDBCreateItem.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: dynamodb.go.create_item.imports.txt
   :dedent: 0
   :language: go

Create the data structure we use to containing the information about the
table item.

.. literalinclude:: dynamodb.go.create_item.struct.txt
   :dedent: 0
   :language: go

Initialize a session that the SDK will use to load
credentials from the shared credentials file *~/.aws/credentials*
and region from the shared configuration file *~/.aws/config*,
and create the |DDB| client.

.. literalinclude:: dynamodb.go.create_item.session.txt
   :dedent: 4
   :language: go

Create a struct with the movie data and marshall that data into a
map of **AttributeValue** objects.

.. literalinclude:: dynamodb.go.create_item.assign_struct.txt
   :dedent: 4
   :language: go

Create the input for **PutItem** and call it.
If an error occurs, print the error and exit.
If no error occurs, print an message that the item was added to the table.

.. literalinclude:: dynamodb.go.create_item.call.txt
   :dedent: 4
   :language: go

See the `complete example <https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/dynamodb/DynamoDBCreateItem.go>`_
on GitHub.
