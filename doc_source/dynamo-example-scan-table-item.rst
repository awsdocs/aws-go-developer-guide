.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-dynamo-example-scan-table-item:

######################################################
Getting |DDBlong| Table Items Using Expression Builder
######################################################

.. meta::
    :description:
        Get Amazon DynamoDB table items using the new Expresion Builder with this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, DynamoDB

The following example uses the |DDB| |nbsp| 
:sdk-go-api-deep:`Scan <service/dynamodb/#DynamoDB.Scan>` operation
to get items with a :code:`rating` greater than **4.0** in the :code:`year` **2013**
in the :code:`Movies` table in your default region.

The example uses the
`Expression Builder
<https://aws.amazon.com/blogs/developer/introducing-amazon-dynamodb-expression-builder-in-the-aws-sdk-for-go/>`_
package released in version 1.11.0 of the |sdk-go| in September 2017.

Create the file *DynamoDBScanItem.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: dynamodb.go.scan_items.imports.txt
   :dedent: 0
   :language: go

Create the data structure we use to contain the information about the
table item.

.. literalinclude:: dynamodb.go.scan_items.struct.txt
   :dedent: 0
   :language: go

Initialize a session that the SDK will use to load
credentials from the shared credentials file *~/.aws/credentials*
and region from the shared configuration file *~/.aws/config*
and create a new |DDB| service client.

.. literalinclude:: dynamodb.go.scan_items.session.txt
   :dedent: 4
   :language: go

Create variables for the minimum rating and year for the table items to
retrieve.

.. literalinclude:: dynamodb.go.scan_items.vars.txt
   :dedent: 4
   :language: go

Create the expression defining the year for which we filter the table items
to retrieve,
and the projection so we get the title, year, and rating for each retrieved item.
Then build the expression.

.. literalinclude:: dynamodb.go.scan_items.expr.txt
   :dedent: 4
   :language: go

Create the inputs for and call **Scan** to retrieve the items from the
table (the movies made in 2013).

.. literalinclude:: dynamodb.go.scan_items.call.txt
   :dedent: 4
   :language: go

Loop through the movies from 2013 and display the
title and rating for those where the rating is greater than 4.0

.. literalinclude:: dynamodb.go.scan_items.process.txt
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/dynamodb/DynamoDBScanItems.go>`_
on GitHub.
