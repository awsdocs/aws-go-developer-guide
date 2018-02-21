.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-iam:

#################################
|IAM| Examples Using the |sdk-go|
#################################

.. meta::
   :description: Use IAM code examples to write your own Go applications.
   :keywords: AWS SDK Go examples, IAM, Identity and Access Management

|IAMlong| (|IAM|) is a web service that enables AWS customers to manage users and user permissions
in AWS. The service is targeted at organizations with multiple users or systems in the cloud that
use AWS products. With |IAM|, you can centrally manage users, security credentials such as access
keys, and permissions that control which AWS resources users can access.

The examples assume you have already set up and configured the SDK (that is, you've imported all
required packages and set your credentials and region). For more information, see :doc:`setting-up`
and :doc:`configuring-sdk`.

You can download complete versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <s3>` repository on GitHub.

.. toctree::
   :titlesonly:
   :maxdepth: 1

   iam-example-managing-users
   iam-example-managing-access-keys
   iam-example-account-aliases
   iam-example-policies
   iam-example-server-certificates
