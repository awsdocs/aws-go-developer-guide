.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _configuring-the-sdk:
                       
########################
Configuring the |sdk-go|
########################

.. meta::
   :description: Configure the |sdk-go| to specify which credentials to use and to which region to send requests.
   :keywords: configuration, specify region, region, credentials, proxy

In the |sdk-go|, you can configure settings for service clients,
such as the log level and maximum number of retries. Most settings are
optional. However, for each service client, you must specify a region
and your credentials. The SDK uses these values to send requests to the
correct AWS Region and sign requests with the correct credentials. You
can specify these values as part of a session or as environment
variables.

.. _specifying-the-region:

Specifying the AWS Region
=========================

When you specify the region, you specify where to send requests, such as
``us-west-2`` or ``us-east-2.`` For a list of regions for each service, see |regions-and-endpoints|_
in the |AWS-gr|.

The SDK does not have a default region.
To specify a region:

- Set the ``AWS_REGION`` environment variable to the default region
- Set the ``AWS_SDK_LOAD_CONFIG`` environment variable to **true**
  to get the region value from the *config* file in the *.aws/* folder in your home directory
- Set the **NewSessionWithOptions** method argument **SharedConfigState** to **SharedConfigEnable** when you create a session
  to get the region value from the *config* file in the *.aws/* folder in your home directory
- Set the region explicitly when you create a session

If you set a region using all of these techniques, the SDK uses the
region you explicitly specified in the session.

The following examples show you how to configure the environment
variable.

**Linux, OS X, or Unix**

.. code-block:: bash

    $ export AWS_REGION=us-west-2

**Windows**

.. code-block:: sh

    > set AWS_REGION=us-west-2

The following snippet specifies the region in a session:

.. code-block:: go

    sess, err := session.NewSession(&aws.Config{Region: aws.String("us-west-2")})


.. _specifying-credentials:

Specifying Credentials
======================

The |sdk-go| requires credentials (an access key and secret access
key) to sign requests to AWS. You can specify your credentials in
several different locations, depending on your particular use case. For
information about obtaining credentials, see :doc:`Setting Up <setting-up>`.

When you initialize a new service client without providing any
credential arguments, the SDK uses the :sdk-go-api-deep:`default credential provider
chain <aws/defaults/#CredChain>` to find AWS credentials. The SDK uses the first provider
in the chain that returns credentials without an error. The default provider chain
looks for credentials in the following order:

1. Environment variables.
2. Shared credentials file.
3. If your application is running on an |EC2| instance, |IAM| role for |EC2|.

The SDK detects and uses the built-in providers automatically, without
requiring manual configurations. For example, if you use |IAM| roles for
|EC2| instances, your applications automatically use the
instance's credentials. You don't need to manually configure credentials
in your application.

As a best practice, AWS recommends that you specify credentials in the
following order:

1. Use |IAM| roles for |EC2| (if your application is running on an
   |EC2| instance).

   |IAM| roles provide applications on the instance temporary security
   credentials to make AWS calls. |IAM| roles provide an easy way to
   distribute and manage credentials on multiple |EC2| instances.

2. Use a shared credentials file.

   This credentials file is the same one used by other SDKs and the |CLI|.
   If you're already using a shared credentials file, you can also use
   it for this purpose.

3. Use environment variables.

   Setting environment variables is useful if you're doing development
   work on a machine other than an |EC2| instance.

4. Hard-code credentials (not recommended).

   Hard-coding credentials in your application can make it difficult to
   manage and rotate those credentials. Use this method only for small
   personal scripts or testing purposes. Do not submit code with
   credentials to source control.

|IAM| Roles for |EC2| Instances
-------------------------------

If you are running your application on an |EC2| instance, you can
use the instance's :ec2-ug:`IAM role <iam-roles-for-amazon-ec2>`
to get temporary security credentials to make calls to AWS.

If you have configured your instance to use |IAM| roles, the SDK uses
these credentials for your application automatically. You don't need to
manually specify these credentials.

.. _shared_credentials_file:

Shared Credentials File
-----------------------

A credential file is a plaintext file that contains your access keys.
The file must be on the same machine on which you're running your
application. The file must be named :file:`credentials` and located in the
:file:`.aws/` folder in your home directory. The home directory can vary by
operating system. In Windows, you can refer to your home directory by
using the environment variable :code:`%UserProfile%`. In Unix-like systems, you
can use the environment variable :code:`$HOME` or :code:`~` (tilde).

If you already use this file for other SDKs and tools (like the |CLI|),
you don't need to change anything to use the files in this SDK. If
you use different credentials for different tools or applications, you
can use *profiles* to configure multiple access keys in the same
configuration file.

Creating the Credentials File
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you don't have a shared credentials file (:file:`.aws/credentials`), you
can use any text editor to create one in your home directory. Add the
following content to your credentials file, replacing
:code:`<YOUR_ACCESS_KEY_ID>` and :code:`<YOUR_SECRET_ACCESS_KEY>` with your
credentials.

.. code-block:: ini

    [default]
    aws_access_key_id = <YOUR_ACCESS_KEY_ID>
    aws_secret_access_key = <YOUR_SECRET_ACCESS_KEY>

The :code:`[default]` heading defines credentials for the default profile,
which the SDK will use unless you configure it to use another profile.

You can also use temporary security credentials by adding the session
tokens to your profile, as shown in the following example:

.. code-block:: ini

    [temp]
    aws_access_key_id = <YOUR_TEMP_ACCESS_KEY_ID>
    aws_secret_access_key = <YOUR_TEMP_SECRET_ACCESS_KEY>
    aws_session_token = <YOUR_SESSION_TOKEN>

Specifying Profiles
~~~~~~~~~~~~~~~~~~~

You can include multiple access keys in the same configuration file by
associating each set of access keys with a profile. For example, in your
credentials file, you can declare multiple profiles, as follows.

.. code-block:: ini

    [default]
    aws_access_key_id = <YOUR_DEFAULT_ACCESS_KEY_ID>
    aws_secret_access_key = <YOUR_DEFAULT_SECRET_ACCESS_KEY>

    [test-account]
    aws_access_key_id = <YOUR_TEST_ACCESS_KEY_ID>
    aws_secret_access_key = <YOUR_TEST_SECRET_ACCESS_KEY>

    [prod-account]
    ; work profile
    aws_access_key_id = <YOUR_PROD_ACCESS_KEY_ID>
    aws_secret_access_key = <YOUR_PROD_SECRET_ACCESS_KEY>

By default, the SDK checks the :code:`AWS_PROFILE` environment variable to
determine which profile to use. If no :code:`AWS_PROFILE` variable is set,
the SDK uses the default profile.

If you have an application named ``myapp`` that uses the SDK, you can
run it with the test credentials by setting the variable to
``test-account myapp``, as shown in the following command.

.. code-block:: sh

    $ AWS_PROFILE=test-account myapp

You can also use the SDK to select a profile by specifying
:code:`os.Setenv("AWS_PROFILE", test-account)` before constructing any
service clients or by manually setting the credential provider, as shown
in the following example.

.. code-block:: go

    sess, err := session.NewSession(&aws.Config{
        Region:      aws.String("us-west-2"),
        Credentials: credentials.NewSharedCredentials("", "test-account"),
    })

In addition, checking if your credentials have been found is fairly easy.

.. code-block:: go

    _, err := sess.Config.Credentials.Get()

If :code:`ChainProvider` is being used, set :code:`CredentialsChainVerboseErrors` to
:code:`true` in the session config.

.. note::
   If you specify credentials in environment variables, the SDK
   will always use those credentials, no matter which profile you specify.

Environment Variables
---------------------

By default, the SDK detects AWS credentials set in your environment and
uses them to sign requests to AWS. That way you don't need to manage
credentials in your applications.

The SDK looks for credentials in the following environment variables:

-  :code:`AWS_ACCESS_KEY_ID`
-  :code:`AWS_SECRET_ACCESS_KEY`
-  :code:`AWS_SESSION_TOKEN` (optional)

The following examples show how you configure the environment variables.

**Linux, OS X, or Unix**

.. code-block:: bash

    $ export AWS_ACCESS_KEY_ID=YOUR_AKID
    $ export AWS_SECRET_ACCESS_KEY=YOUR_SECRET_KEY
    $ export AWS_SESSION_TOKEN=TOKEN

**Windows**

.. code-block:: sh

    > set AWS_ACCESS_KEY_ID=YOUR_AKID
    > set AWS_SECRET_ACCESS_KEY=YOUR_SECRET_KEY
    > set AWS_SESSION_TOKEN=TOKEN

Hard-Coded Credentials in an Application (Not Recommended)
----------------------------------------------------------

.. warning::
   Do not embed credentials inside an application. Use this
   method only for testing purposes.

You can hard-code credentials in your application by passing the access
keys to a configuration instance, as shown in the following snippet.

.. code-block:: go

    sess, err := session.NewSession(&aws.Config{
        Region:      aws.String("us-west-2"),
        Credentials: credentials.NewStaticCredentials("AKID", "SECRET_KEY", "TOKEN"),
    })

Other Credentials Providers
---------------------------

The SDK provides other methods for retrieving credentials in the
:code:`aws/credentials` package. For example, you can retrieve temporary
security credentials from |STSlong| or credentials from
encrypted storage. For more information, see :sdk-go-api-deep:`Credentials
<aws/credentials/>`.

.. _configuring-a-proxy:

Configuring a Proxy
===================

If you cannot directly connect to the internet, you can use Go-supported
environment variables (``HTTP_PROXY``) or create a custom HTTP client to
configure your proxy. Use the
:sdk-go-api-deep:`Config.HTTPClient <aws/#Config.WithHTTPClient>`
struct to specify a custom HTTP client. For more information about how
to create an HTTP client to use a proxy, see the
`Transport <https://golang.org/pkg/net/http/#Transport>`_ struct in
the Go ``http`` package.

.. _logging:

Logging Service Calls
=====================

You can enable logging in a client by setting the :code:`LogLevel` in a configuration instance,
as shown in the following snippet, which sets the log level to :code:`LogDebugWithHTTPBody` for a new |DDB| client.

.. code-block:: go

    svc := dynamodb.New(sess, aws.NewConfig().WithLogLevel(aws.LogDebugWithHTTPBody))

See :sdk-go-api-deep:`LogLevelType <aws/#LogLevelType>` for the different log level values.

.. _custom_endpoint:

Creating a Custom Endpoint
==========================

In most cases you use the endpoint that is pre-configured for a service.
However, you can specify a custom endpoint, such as for pre-release versions of the service,
as shown in the following snippet,
which sets the :code:`Endpoint` to :code:`https://test.us-west-2.amazonaws.com` for a new |DDB| client.

.. code-block:: go

   svc := dynamodb.New(sess, &aws.Config{Endpoint: aws.String("https://test.us-west-2.amazonaws.com")})

See :sdk-go-api-deep:`aws.Config <aws/#Config>` for details.

.. Just for this CSM section

.. |language| replace:: Go   
.. |sdk| replace:: |sdk-go|
.. |CSM| replace:: SDK Metrics
.. |CSMlong| replace:: AWS SDK Metrics for Enterprise Support
.. |CSMmerge| replace:: AWS SDK Metrics for Enterprise Support (SDK Metrics)
.. |CreatePolicy| replace:: :sdk-go-api-deep:`CreatePolicy <service/iam/#IAM.CreatePolicy>`
.. |CreateRole| replace:: :sdk-go-api-deep:`CreateRole <service/iam/#IAM.CreateRole>`
.. |AttachRolePolicy| replace:: :sdk-go-api-deep:`AttachRolePolicy <service/iam/#IAM.AttachRolePolicy>`

.. _csm_metrics:

Using |CSM| in the |sdk|
========================

.. meta::
   :description: Configure an agent for AWS SDK Metrics for Enterprise Support with the |sdk|.
   :keywords: |sdk|, AWS SDK Metrics for Enterprise Support with |language|, use |language| to monitor AWS Services

|CSMmerge| enables Enterprise customers to monitor their AWS service use.
|CSM| can help to identify the health of their AWS services
and diagnose latency caused by reaching their account usage limits or a service outage.

|CSM| for AWS SDKs delivers telemetry from within the AWS SDKs that makes it possible for AWS to know
when applications experience connection issues to AWS endpoints, and gain insight into why the issues occur.
Telemetry helps AWS reduce the time to resolve problems that impact your application's access to AWS services.

|CSM| monitors actions by using a |CWlong| agent running in the same environment as a client application
that is using the |sdk|.
The following steps to set up |CSM| focus on an |EC2| Linux instance.
|CSM| is also available for your production environments if you enable it while configuring the |sdk|. 

To utilize |CSM|, run the most current version of the |CW| agent.
Learn how to :CW-dg:`Configure the CloudWatch Agent for SDK Metrics<Configure-CloudWatch-Agent-SDK-Metrics.>` in the |CW-dg|.

To set up |CSM| with the |sdk|, follow these instructions:

1. Create an application with an |sdk| client to use an AWS service.
2. Host your project on an |EC2| instance or in your local environment.
3. Install and use the most current version of the |sdk|.
4. Install and configure an |CW| agent on an EC2 instance or in your local environment.
5. :ref:`csm-set-permissions`
6. :ref:`csm-enable-agent`
7. :ref:`csm-view-metrics`

For more information, see the following:

* :ref:`csm-update-agent`
* :ref:`csm-disable-agent`

.. _csm-set-permissions:

Configure |CSM| to Collect and Send Metrics
-------------------------------------------

You can configure |CSM| so that it collects and sends metrics by using the
|sdk| or the |IAM| console,
as explained in the following sections.

.. _csm_setup_sdk:

Set Up Access Permissions Using the |sdk|
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create an IAM role for the instance that has permission for |SSMlong| and |CSM|.

First create a policy using |CreatePolicy|.
Then create a role using |CreateRole|.
Finally, attach the policy you created to your new role with |AttachRolePolicy|.

.. literalinclude:: example_code/iam/iam_createcsmrole.go
   :lines: 14-
   :language: go

.. :start-after: snippet-start:[iam.go.create_csm_role]
.. :end-before: snippet-end:[iam.go.create_csm_role]

.. _csm_setup_console:
                
Set Up Access Permissions Using the |IAM| Console
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can also use the IAM console to create a role.

.. topic:: To create a role using the IAM console

1. Go to the `IAM console <https://console.aws.amazon.com/iam>`_, and create a role to use |EC2|.
2. In the navigation pane, choose **Roles**.
3. Choose **Create Role**.
4. Choose **AWS Service**, and then choose **EC2**.
5. Choose **Next: Permissions**.
6. Under **Attach permissions policies**, choose **create policy**.
7. For :guilabel:`Service`, choose **SSM**. For :guilabel:`Actions`, choose :code:`GetParameter`.
   For resources, specify the |CW| agent created in the previous section.
8. Add an additional permission.
9. Select **Choose a service**, and then **Enter service manually**.
   For :guilabel:`Service`, enter :code:`sdkmetrics`.
   Select all :code:`sdkmetrics` actions and all resources, and then choose **Review Policy**.
10. Name the :guilabel:`Role` :code:`AmazonCSM`, and add a description.
11. Choose **Create Role**.

.. _csm-enable-agent:

Enabling |CSM| for the |sdk|
----------------------------

|CSM| has the following default parameters.

.. code-block:: ini

    //default values
     [
         'enabled' => false,
         'port' => 31000,
         'client_id' => ''
     ]

You can enable |CSM| by setting an environment variable or by setting a value in the AWS Shared config file,
as explained in the following sections.

.. _enable_csm_env_var:

Enable |CSM| Using an Environment Variable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To enable |CSM|, set the `AWS_CSM_ENABLED` environmental variable.

.. code-block:: ini

    AWS_CSM_ENABLED=true

Other configuration settings are available.
For more information about using shared files, see
:doc:`Shared Credentials File <shared_credentials_file>`.

Note: Enabling |CSM| does not configure your credentials to use an AWS service. 
To do that, see
:doc:`Specifying Credentials<specifying-credentials>`.

.. _enable_csm_config_file:

AWS Shared Config File
~~~~~~~~~~~~~~~~~~~~~~

If no CSM configuration is found in the environment variables, the SDK looks for your customized AWS profile field.
Then it checks the :code:`aws_csm` profile.
To enable |CSM|,
add :code:`csm_enabled` to the shared config file `~/.aws/config`.

.. code-block:: ini

    [custom_profile_from_aws_profile]
    csm_enabled = true

    [aws_csm]
    csm_enabled = true

Other configuration settings are available.
For more information about using AWS Shared files, see
:doc:`Specifying Credentials <specifying_credentials>`.

Note: Enabling |CSM| does not configure your credentials to use an AWS service.

.. _csm-view-metrics:

Viewing Metrics in |CW|
-----------------------

For Enterprise users, the agent automatically captures data about each client operation and passes the information to |CW|.

.. topic:: To access your metrics

1. Go to the `CloudWatch console <http://console.aws.amazon.com/cloudwatch>`_.
2. In the navigation pane, choose :guilabel:`Metrics`.
3. Choose :guilabel:`SDKMetrics`.
4. View the metrics.

.. You can use the |sdk| to get the monitored API call and call attempt events for an operation,
   as shown in the following example,
   which gets the events for *MyObject* in the |S3| bucket *MyBucket*.

   code-block:: go

   ???

.. _csm-update-agent:

Updating a |CW| Agent
---------------------

To change the port or client ID,
set the values and then restart any AWS jobs that are currently active.
You can set the values by using environment variables or in the shared configuration file,
as explained in the following sections.

.. _csm-update-agent-env-var:

Setting |CSM| Values Using Environment Variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

|CSM| assigns a client ID to your application environment that is a
searchable index point in the |CW| dashboard.
To add a customized client ID string, add
`AWS_CSM_CLIENT_ID=[some_string]` to the host's environment variables.

Most services use the default port.
But if your service requires a unique port ID,
add `AWS_CSM_PORT=[port_number]`, to the host's environment variables.

.. code-block:: ini

    AWS_CSM_ENABLED=true
    AWS_CSM_CLIENT_ID=myAppName
    AWS_CSM_PORT=1234

.. _csm-update-agent-env-var:

Setting |CSM| Values Using the AWS Shared Config File
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

|CSM| assigns a client ID to your application environment that is a
searchable index point in the |CW| dashboard.  To add a customized client ID string, add
`csm_client_id = [some_string]` to `~/.aws/config`.

Most services use the default port.
If your service requires a unique port ID,
add `csm_port = [port_number]` to `~/.aws/config`.

.. code-block:: ini

    [custom_profile_from_aws_profile]
    csm_enabled = false
    csm_client_id = myAppName
    csm_port = 1234

    [aws_csm]
    csm_enabled = false
    csm_client_id = myAppName
    csm_port = 1234

.. _restart_csm:
    
Restarting |CSM|
----------------

To restart |CSM|, stop and start the agent:

.. code-block:: ini

    amazon-cloudwatch-agent-ctl -a stop
    amazon-cloudwatch-agent-ctl -a start

.. _csm-disable-agent:

Disabling |CSM|
---------------

To disable |CSM|, set the `AWS_CSM_ENABLED` environment variable to `false`,
or set the `csm_enabled` value to `false` your AWS Shared config file `~/.aws/config`.
Then restart your |CW| agent so that the changes can take effect.

**Environment Variable**

.. code-block:: ini

    AWS_CSM_ENABLED=false

**AWS Shared Config File**

.. note:: Environment variables override the AWS Shared config file. If |CSM| is enabled in the environment variables, the |CSM| remains enabled.

.. code-block:: ini

    [custom_profile_from_aws_profile]
    csm_enabled = false

    [aws_csm]
    csm_enabled = false

To stop |CSM|, use the following command.

.. code-block:: ini

    amazon-cloudwatch-agent-ctl -a stop
