.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-dynamo-example-read-table-item:

###############################
Reading an |DDBlong| Table Item
###############################

.. meta::
    :description:
        Display information about an item in an Amazon DynamoDB tables using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, DynamoDB

The following example uses the |DDB| |nbsp| 
:sdk-go-api-deep:`GetItem <service/dynamodb/#DynamoDB.GetItem>` operation
to retrieve information about the item with the :code:`year` **2015** and
:code:`title`  **The Big New Movie** in the :code:`movies` table in 
your default region.

Create the file *DynamoDBReadItem.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: dynamodb.go.read_item.imports.txt
   :dedent: 0
   :language: go

Create the data structure we use to contain the information about the
table item.

.. literalinclude:: dynamodb.go.read_item.struct.txt
   :dedent: 0
   :language: go

Initialize a session that the SDK will use to load
credentials from the shared credentials file *~/.aws/credentials*
and region from the shared configuration file *~/.aws/config*
and create a new |DDB| service client.

.. literalinclude:: dynamodb.go.read_item.session.txt
   :dedent: 4
   :language: go

Call **GetItem** to get the item from the table.
If we encounter an error, print the error message.
Otherwise, display information about the item.

.. literalinclude:: dynamodb.go.read_item.call.txt
   :dedent: 4
   :language: go

Unmarshall the return value and if the title is not empty,
indicating we got the item,
display the year, title, plot, and rating.

.. literalinclude:: dynamodb.go.read_item.unmarshall.txt
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/dynamodb/DynamoDBReadItem.go>`_
on GitHub.
