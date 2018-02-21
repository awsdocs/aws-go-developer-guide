.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _polly-example-describe-voices:

########################
Getting a List of Voices
########################

.. meta::
    :description:
        Get the list of voices that are available for use when requesting speech synthesis using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, Amazon Polly

This example uses the
:sdk-go-api-deep:`DescribeVoices <service/polly/#Polly.DescribeVoices>` operation
to get the list of voices
in the :code:`us-west-2` region.

Choose :code:`Copy` to save the code locally.

Create the file *pollyDescribeVoices.go*.
Import the packages used in the example.

.. literalinclude:: ./example_code/polly/pollyDescribeVoices.go
   :lines: 17-24
   :dedent: 0
   :language: go

Initialize a session that the SDK will use to load credentials
from the shared credentials file :file:`~/.aws/credentials`,
load your configuration from the shared configuration file :file:`~/.aws/config`,
and create an Amazon Polly client.

.. literalinclude:: ./example_code/polly/pollyDescribeVoices.go
   :lines: 29-32, 34
   :dedent: 4
   :language: go

Create the input for and call :code:`DescribeVoices`.

.. literalinclude:: ./example_code/polly/pollyDescribeVoices.go
   :lines: 37, 39
   :dedent: 4
   :language: go

Display the name and gender of the voices.

.. literalinclude:: ./example_code/polly/pollyDescribeVoices.go
   :lines: 46-50
   :dedent: 4
   :language: go
	      
See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/polly/pollyDescribeVoices.go>`_
on GitHub.
