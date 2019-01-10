.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _authorize_metrics:

########################################################
Authorize |CSM| to Collect and Send Metrics in the |sdk|
########################################################

To collect metrics from AWS SDKs using |CSM| for Enterprise Support,
Enterprise customers must create an |IAM| Role that gives |CW| agent permission
to gather data from their |EC2| instance or production environment.

Use the following |language| code sample or the AWS Console to create an
|IAM| Policy and Role for an |CW| agent to access |CSM| in your environment.

Learn more about using |CSM| with |sdk| in :ref:`setup-metrics`.
For more information about |CSM|,
see |CW_IAM_CSM| in the *|CWlong| User Guide*.

.. _setup_access_permissions_sdk:

Set Up Access Permissions Using the |sdk|
=========================================

Create an |IAM| role for the instance that has permission for |EC2| Systems Manager and |CSM|.

First, create a policy using |CreatePolicy|.
Then create a role using |CreateRole|.
Finally, attach the policy you created to your new role with |AttachRolePolicy|.

.. literalinclude:: iam.go.create_csm_role
   :language: go

.. _setup_access_permissions_console:

Set Up Access Permissions by Using the |IAM| Console
====================================================

Alternatively, you can use the |IAM| console to create a role.

- Go to the |IAM| console, and create a role to use |EC2|.

- In the navigation pane, choose **Roles**.

- Choose **Create Role**.

- Choose **AWS Service**, and then **EC2**.

- Choose **Next: Permissions**.

- Under **Attach permissions policies**, choose **create policy**.

- For **Service**, choose **Systems Manager**.
  For **Actions**, expand **Read**, and choose ``GetParameters``.
  For resources, specify your |CW| agent.

- Add additional permission.

- Select **Choose a service**, and then **Enter service manually**.
  For **Service**, enter ``sdkmetrics``.
  Select all ``sdkmetrics`` actions and all resources, and then choose **Review Policy**.

- Name the **Role** ``AmazonSDKMetrics``, and add a description.

- Choose **Create Role**.
