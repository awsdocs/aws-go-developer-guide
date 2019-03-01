.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-sns-example-publish:

################################################
Sending a Message to All |SNS| Topic Subscribers
################################################

.. meta::
    :description: Send messages to all |SNS| topic subscribers using this |sdk-go| code example.
    :keywords: |sdk-go| code examples, SNS

The following example sends the message supplied on the command line to all subscribers to the |SNS| topic with the ARN
specified on the command line.

.. literalinclude:: sns.go.publish.txt
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/sns/SnsPublish.go>`_
on GitHub.
