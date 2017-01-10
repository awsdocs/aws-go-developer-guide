.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-dynamodb:

##########################
|DDBlong| with Go Examples
##########################

.. meta::
   :description: Use DynamoDB code examples to write your own Go applications.
   :keywords: AWS SDK for Go examples, DynamoDB

The |sdk-go| examples can integrate |DDBlong| into your Go applications.
The examples assume you have already set up and configured the SDK (that
is, you have imported all required packages and set your credentials and region). For more information, see :doc:`setting-up` and :doc:`configuring-sdk`.

.. _list-tables:

Listing Tables
==============

The following example uses the |DDB|
:sdk-go-api-deep:`ListTables <service/dynamodb/#DynamoDB.ListTables>` operation
to list all tables for the region you specified (``us-west-2``).


.. literalinclude:: example_code/dynamodb/list_tables.go
   :dedent: 4
   :lines: 15-
