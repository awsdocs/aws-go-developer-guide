.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-dynamo-example-create-table:

###########################
Creating an |DDBlong| Table
###########################

.. meta::
   :description: Create a DynamoDB table using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, DynamoDB

The following example uses the |DDB| |nbsp| 
:sdk-go-api-deep:`CreateTable <service/dynamodb/#DynamoDB.CreateTable>` operation
to create the table **Music** your default region.

Create the file *DynamoDBCreateTable.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: dynamodb.go.create_table.imports.txt
   :dedent: 0
   :language: go

Initialize a session that the SDK will use to load
credentials from the shared credentials file *~/.aws/credentials*
and region from the shared configuration file *~/.aws/config*,
and create the |DDB| client.

.. literalinclude:: dynamodb.go.create_table.session.txt
   :dedent: 4
   :language: go

Call **CreateTable**.
If an error occurs, print the error and exit.
If no error occurs, print an message that the table was created.

.. literalinclude:: dynamodb.go.create_table.call.txt
   :dedent: 4
   :language: go

See the `complete example <https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/dynamodb/DynamoDBCreateTable.go>`_
on GitHub.
