.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _polly-example-list-lexicons:

##########################
Getting a List of Lexicons
##########################

.. meta::
    :description:
        Get the list of pronunciation lexicons stored in an AWS Region using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, Amazon Polly

This example uses the
:sdk-go-api-deep:`ListLexicons <service/polly/#Polly.ListLexicons>` operation
to get the list of lexicons in the :code:`us-west-2` region.

Choose :code:`Copy` to save the code locally.

Create the file *pollyListLexicons.go*.
Import the packages used in the example.

.. literalinclude:: ./example_code/polly/pollyListLexicons.go
   :lines: 17-23
   :dedent: 0
   :language: go

Initialize a session that the SDK will use to load credentials
from the shared credentials file :file:`~/.aws/credentials`,
load your configuration from the shared configuration file :file:`~/.aws/config`,
and create an Amazon Polly client.

.. literalinclude:: ./example_code/polly/pollyListLexicons.go
   :lines: 28-31, 33
   :dedent: 4
   :language: go

Call :code:`ListLexicons` and display the name, alphabet, and language code of each lexicon.

.. literalinclude:: ./example_code/polly/pollyListLexicons.go
   :lines: 35, 41-47
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/polly/pollyListLexicons.go>`_
on GitHub.
