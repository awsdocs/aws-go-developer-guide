.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-dynamo-example-create-table-item:

###############################################
Creating |DDBlong| Table Items from a JSON File
###############################################

.. meta::
   :description: Create DynamoDB table items from a JSON file using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, DynamoDB

The following example uses the |DDB| |nbsp| 
:sdk-go-api-deep:`PutItem <service/dynamodb/#DynamoDB.PutItem>` operation
in a loop to create the items defined in *movie_data.json* file in the :code:`Movies` table in 
your default region.

Create the file *DynamoDBLoadItems.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: dynamodb.go.load_items.imports.txt
   :dedent: 0
   :language: go

Create the data structure we use to contain the information about the
table item.

.. literalinclude:: dynamodb.go.load_items.struct.txt
   :dedent: 0
   :language: go

Create a function to get the table items from the JSON file.

.. literalinclude:: dynamodb.go.load_items.func.txt
   :dedent: 0
   :language: go

Initialize a session that the SDK will use to load
credentials from the shared credentials file *~/.aws/credentials*
and region from the shared configuration file *~/.aws/config*
and create a new |DDB| service client.

.. literalinclude:: dynamodb.go.load_items.session.txt
   :dedent: 4
   :language: go

Call **getItems** to get the items.
Loop through each item,
marshall that data into a
map of **AttributeValue** objects,
add the item to the :code:`Movies` table,
and print out the title and year of the movie added to the table.

.. literalinclude:: dynamodb.go.load_items.call.txt
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/dynamodb/DynamoDBLoadItems.go>`_
and a `sample JSON file 
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/dynamodb/movie_data.json>`_
on GitHub.
