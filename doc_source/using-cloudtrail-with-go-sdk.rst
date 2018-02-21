.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-cloudtrail:

####################################
|CTLong| Examples Using the |sdk-go|
####################################

.. meta::
   :description: Use CloudTrail code examples to write your own Go applications.
   :keywords: AWS SDK for Go code examples, |CTlong|

|CT| is an AWS service that helps you enable governance, compliance, and
operational and risk auditing of your AWS account.
Actions taken by a user, role, or an AWS service are recorded as events in |CT|.
Events include actions taken in the AWS Management Console, AWS Command
Line Interface, and AWS SDKs and APIs.

The examples assume you have already set up and configured the SDK
(that is, you've imported all required packages and set your credentials
and region).
For more information, see :doc:`setting-up` and :doc:`configuring-sdk`.

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <cloudtrail>` repository on GitHub.

.. toctree::
   :maxdepth: 1

   cloudtrail-example-describe-trails
   cloudtrail-example-create-trail
   cloudtrail-example-lookup-events
   cloudtrail-example-delete-trail

