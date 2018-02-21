.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-dynamo-example-delete-table-item:

################################
Deleting an |DDBlong| Table Item
################################

.. meta::
    :description:
        Delete an Amazon DynamoDB table item using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, DynamoDB

The following example uses the |DDB|
:sdk-go-api-deep:`UpdateItem <service/dynamodb/#DynamoDB.UpdateItem>` operation
to delete the item with the :code:`year` **2015** and
:code:`title`  **The Big New Movie**
from the :code:`Movies` table in the :code:`us-west-2` region.

Create the file *dynamodb_update_item.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/dynamodb/delete_item.go
   :lines: 17-23
   :dedent: 0
   :language: go

Initialize the session that the SDK uses to credentials
from the shared credentials file *~/.aws/credentials,
and create a new |DDB| service client.

.. literalinclude:: example_code/dynamodb/delete_item.go
   :lines: 28-33
   :dedent: 4
   :language: go

Call **DeleteItem** to delete the item from the table.
If we encounter an error, print the error message.
Otherwise, display a message that the item was deleted.

.. literalinclude:: example_code/dynamodb/delete_item.go
   :lines: 35-55
   :dedent: 4
   :language: go

See the `complete example 
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/dynamodb/delete_item.go>`_
on GitHub.
