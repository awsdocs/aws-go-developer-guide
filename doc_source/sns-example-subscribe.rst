.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-sns-example-subscribe:

###########################
Subscribe to an |SNS| Topic
###########################

.. meta::
    :description: Create a subscription in an Amazon SNS topic using the |sdk-go|.
    :keywords: |sdk-go| code examples, SNS, subscribe

The following example creates a subscription to the topic with the supplied ARN
for the user with the supplied email address in your default region,
and displays the resulting ARN.

.. literalinclude:: sns.go.subscribe.txt
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/sns/SnsSubscribe.go>`_
on GitHub.
