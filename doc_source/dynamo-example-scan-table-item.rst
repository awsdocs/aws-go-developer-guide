.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

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

The following example uses the |DDB|
:sdk-go-api-deep:`Scan <service/dynamodb/#DynamoDB.Scan>` operation
to get items with a :code:`rating` greater than **8.0** in the :code:`year` **2011**
in the :code:`Movies` table in the :code:`us-west-2` region.

The example uses the
`Expression Builder
<https://aws.amazon.com/blogs/developer/introducing-amazon-dynamodb-expression-builder-in-the-aws-sdk-for-go/>`_
package released in version 1.11.0 of the |sdk-go| in September 2017.

Create the file *dynamodb_scan_item.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/dynamodb/scan_items.go
   :lines: 17-26
   :dedent: 0
   :language: go

Create the data structures we use to contain the information about the
table item.

.. literalinclude:: example_code/dynamodb/scan_items.go
   :lines: 29-38
   :dedent: 0
   :language: go

Create variables for the minimum rating and year for the table items to
retrieve.

.. literalinclude:: example_code/dynamodb/scan_items.go
   :lines: 42-43
   :dedent: 4
   :language: go

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials,
and create a new |DDB| service client.

.. literalinclude:: example_code/dynamodb/scan_items.go
   :lines: 47-58
   :dedent: 4
   :language: go

Create the expression defining the year for which we filter the table items
to retrieve,
and the projection so we get the title, year, and rating for each retrieved item.
Then build the expression.

.. literalinclude:: example_code/dynamodb/scan_items.go
   :lines: 62-70
   :dedent: 4
   :language: go

Create the inputs for and call **Scan** to retrieve the items from the
table (the movies made in 2011).

.. literalinclude:: example_code/dynamodb/scan_items.go
   :lines: 79-88
   :dedent: 4
   :language: go

Loop through the movies from 2011 and display the
title and rating for those where the rating is greater than 8.0

.. literalinclude:: example_code/dynamodb/scan_items.go
   :lines: 98-119
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/dynamodb/scan_items.go>`_
on GitHub.
