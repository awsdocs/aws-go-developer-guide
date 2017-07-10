.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _lambda-go-example-configure-function-for-notification:

#####################################################
Configuring a |LAM| Function to Receive Notifications
#####################################################

The following example configures the |LAM| function :code:`my-notification-function` in the :code:`us-west-2` region to accept notifications from the resource with the ARN :code:`my-resource-arn`.

.. literalinclude:: ./example_code/lambda/aws-go-sdk-lambda-example-configure-function-for-notification.go
   :lines: 12-37
   :dedent: 0
   :language: go
