.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-dynamo-example-update-table-item:

################################
Updating an |DDBlong| Table Item
################################

.. meta::
    :description:
        Update an Amazon DynamoDB table item using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, DynamoDB

The following example updates the rating to **0.1** for the item with the :code:`year` **2015** and
:code-go:`title`  **The Big New Movie**
in the :code-go:`Movies` table in the :code-go:`us-west-2` region.

.. literalinclude:: ./example_code/dynamodb/update_item.go
   :lines: 13-
   :dedent: 0
   :language: go

See the `complete example <https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/dynamodb/update_item.go>`_
on GitHub.
