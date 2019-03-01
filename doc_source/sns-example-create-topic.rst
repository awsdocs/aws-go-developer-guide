.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-sns-example-create-topic:

#######################
Creating an |SNS| Topic
#######################

.. meta::
    :description: Create an Amazon SNS topic using this |sdk-go| code example.
    :keywords: |sdk-go| code examples, SNS, create topic

The following example creates a topic with the name from the command line,
in your default region, and displays the resulting topic ARN.

.. literalinclude:: sns.go.create_topic.txt
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/sns/SnsCreateTopic.go>`_
on GitHub.
