.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

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

The following example deletes item with the :code:`year` **2015** and
:code:`title`  **The Big New Movie**
from the :code:`Movies` table in the :code:`us-west-2` region.

.. literalinclude:: ./example_code/dynamodb/delete_item.go
   :lines: 13-
   :dedent: 0
   :language: go

See the `complete example <https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/dynamodb/delete_item.go>`_
on GitHub.
