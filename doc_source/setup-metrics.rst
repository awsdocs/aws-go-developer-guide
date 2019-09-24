.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _setup_metrics:

#########################
Set up |CSM| in the |sdk|
#########################

.. meta::
   :description: Configure an agent for |CSM| for Enterprise Support with the |sdk|.
   :keywords: |sdk|, |CSM| for Enterprise Support with |language|, use |language| to monitor AWS Services

The following steps demonstrate how to set up |CSM| for the |sdk|.
These steps pertain to an |EC2| instance running Amazon Linux for a client application that is using the |sdk|.
|CSM| is also available for your production environments if you enable it while configuring the |sdk|.

To use |CSM|, run the latest version of the |CW| agent.

.. Learn how to |CW_IAM_CSM| in the Amazon |CW| User Guide.

For details about |IAM| Permissions for |CSM|, see :doc:`authorize-metrics`.

To set up |CSM| with the |sdk|:

#. Create an application with an |sdk| client to use an AWS service.

#. Host your project on an |EC2| instance or in your local environment.

#. Install and use the latest version of the |sdk|.

#. Install and configure a |CW| agent on an |EC2| instance or in your local environment.

#. Authorize |CSM| to collect and send metrics.

#. :ref:`enable_sdk_metrics`.

For more information, see:

- :ref:`update_cw_agent`.

- :ref:`disable_sdk_metrics`.

.. _enable_sdk_metrics:

Enable |CSM| for the |sdk|
==========================

By default, |CSM| is turned off, and the port is set to 31000.
The following are the default parameters.

.. code-block:: sh

   //default values
   [
     'enabled' => false,
     'port' => 31000,
   ]

Enabling |CSM| is independent of configuring your credentials to use an AWS service.

You can enable |CSM| by setting environment variables or by using the AWS Shared config file.

.. _enable_csm_option_1:

Option 1: Set Environment Variables
-----------------------------------

The SDK first checks the profile specified in the environment variable under ``AWS_PROFILE``
to determine if |CSM| is enabled.

To turn on |CSM|, add the following to your environmental variables.

``export AWS_CSM_ENABLED=true``

Other configuration settings are available, 
see :ref:`update_cw_agent` for details.
For more information about using shared files, see the environment variables
information in :doc:`configuring-sdk`.

.. Note::

   Enabling |CSM| does not configure your credentials to use an AWS service.
   To do that, see :ref:`specifying-credentials`.

.. _enable_csm_option_2:

Option 2: AWS Shared Config File
--------------------------------

If no |CSM| configuration is found in the environment variables,
the |sdk| looks for your customized AWS profile field.
Then it checks the ``aws_csm`` profile.
To enable |CSM|, add ``csm_enabled`` to the shared config file *~/.aws/config*.

.. code-block:: sh
   
    [default]
    csm_enabled = true

    [profile aws_csm]
    csm_enabled = true

Other configuration settings are available, 
see :ref:`update_cw_agent` for details.
For more information about using shared files, see the environment variables
information in :doc:`configuring-sdk`.

.. Note::

   Enabling |CSM| does not configure your credentials to use an AWS service.
   To do that, see :ref:`specifying-credentials`.

.. _update_cw_agent:

Update a |CW| Agent
===================

To make changes to the port number or the Client ID,
set the values and then restart any AWS jobs that are currently active.

.. _update_cw_agent_option1:

Option 1: Set Environment Variables
-----------------------------------

Most AWS services use the default port. But if the service you want |CSM| to monitor uses a unique port,
add ``AWS_CSM_PORT=[PORT-NUMBER]``, where PORT-NUMBER is the port number, to the host's environment variables.

.. code-block:: sh

    export AWS_CSM_ENABLED=true
    export AWS_CSM_PORT=1234
    export AWS_CSM_CLIENT_ID="My Application"

.. _update_cw_agent_option2:

Option 2: AWS Shared Config File
--------------------------------

Most services use the default port.
If your service requires a unique port ID,
add ``AWS_CSM_PORT=[PORT-NUMBER]``, where PORT-NUMBER is the port number, to *~/.aws/config*.

.. code-block:: sh

    [default]
    csm_enabled = false
    csm_port = 1234
    csm_client_id = "My Application"

    [profile aws_csm]
    csm_enabled = false
    csm_port = 1234
    csm_client_id = "My Application"

.. _restart_csm:

Restart |CSM|
-------------

To restart a job, run the following commands.

.. code-block:: sh

    amazon-cloudwatch-agent-ctl -a stop;
    amazon-cloudwatch-agent-ctl -a start;

.. _disable_sdk_metrics:

Disable |CSM|
=============

To turn off |CSM|, set ``csm_enabled`` to **false** in your environment variables
or in your AWS Shared config file *~/.aws/config*.
Then restart your |CW| agent so that the changes can take effect.

.. _set_csm_enabled_false:

Set ``csm_enabled`` to **false**
--------------------------------

.. _set_csm_enabled_false_option1:

Option 1: Environment Variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``export AWS_CSM_ENABLED=false``

.. _set_csm_enabled_false_option1:

Option 2: AWS Shared Config File
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. note:: 

   Environment variables override the AWS Shared config file.
   If |CSM| is enabled in the environment variables, the |CSM| remains enabled.

.. code-block:: sh

    [default]
    csm_enabled = false

    [profile aws_csm]
    csm_enabled = false

.. _stop_csm_restart_cw_agent:

Stop |CSM| and Restart |CW| Agent
---------------------------------

To disable |CSM|, use the following command.

``sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a stop && echo "Done"``

If you are using other |CW| features, restart |CW| with the following command.

``amazon-cloudwatch-agent-ctl -a start;``
