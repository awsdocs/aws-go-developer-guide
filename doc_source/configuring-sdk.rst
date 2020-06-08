.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

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
   :description: Configure the |sdk-go| to specify which credentials to use and to which AWS Region to send requests.
   :keywords: configuration, specify region, region, credentials, proxy

In the |sdk-go|, you can configure settings for service clients,
such as the log level and maximum number of retries. Most settings are
optional. However, for each service client, you must specify an AWS Region
and your credentials. The SDK uses these values to send requests to the
correct Region and sign requests with the correct credentials. You
can specify these values as part of a session or as environment
variables.

.. _creating-sesstion:

Creating a Session
==================

Before you can create a service client you must create a session,
which is part of the **github.com/aws/aws-sdk-go/aws/session** package.

There are a number of ways of configuring a session but the following are the most common.

Create a session using the default Region and credentials:

.. code:: go

   import (
       "github.com/aws/aws-sdk-go/aws"
       "github.com/aws/aws-sdk-go/aws/session"
   )

   // ...
   
   sess := session.Must(session.NewSessionWithOptions(session.Options{
       SharedConfigState: session.SharedConfigEnable,
   }))

Create a session in **us-west-2**:

.. code:: go
          
   import (
       "github.com/aws/aws-sdk-go/aws"
       "github.com/aws/aws-sdk-go/aws/session"
   )

   // ...
   
   sess, err := session.NewSession(&aws.Config{
       Region: aws.String("us-west-2")},
   )

See `session
<https://docs.aws.amazon.com/sdk-for-go/api/aws/session>`_   
for additional information.

.. _specifying-the-region:

Specifying the AWS Region
=========================

When you specify the Region, you specify where to send requests, such as
``us-west-2`` or ``us-east-2.`` For a list of Regions for each service, see |regions-and-endpoints|_
in the |AWS-gr|.

The SDK does not have a default Region.
To specify a Region:

- Set the ``AWS_REGION`` environment variable to the default Region
- Set the ``AWS_SDK_LOAD_CONFIG`` environment variable to **true**
  to get the Region value from the *config* file in the *.aws/* folder in your home directory
- Set the **NewSessionWithOptions** method argument **SharedConfigState** to **SharedConfigEnable** when you create a session
  to get the Region value from the *config* file in the *.aws/* folder in your home directory
- Set the Region explicitly when you create a session

If you set a Region using all of these techniques, the SDK uses the
Region you explicitly specified in the session.

The following examples show you how to configure the environment
variable.

**Linux, OS X, or Unix**

.. code-block:: bash

    $ export AWS_REGION=us-west-2

**Windows**

.. code-block:: sh

    > set AWS_REGION=us-west-2

The following snippet specifies the Region in a session:

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
3. If your application uses an ECS task definition or RunTask API operation,
   |IAM| role for tasks.
4. If your application is running on an |EC2| instance, |IAM| role for |EC2|.

The SDK detects and uses the built-in providers automatically, without
requiring manual configurations. For example, if you use |IAM| roles for
|EC2| instances, your applications automatically use the
instance's credentials. You don't need to manually configure credentials
in your application.

As a best practice, AWS recommends that you specify credentials in the
following order:

1. Use |IAM| roles for tasks if your application uses an ECS task definition or RunTask API operation.

2. Use |IAM| roles for |EC2| (if your application is running on an
   |EC2| instance).

   |IAM| roles provide applications on the instance temporary security
   credentials to make AWS calls. |IAM| roles provide an easy way to
   distribute and manage credentials on multiple |EC2| instances.

3. Use a shared credentials file.

   This credentials file is the same one used by other SDKs and the |CLI|.
   If you're already using a shared credentials file, you can also use
   it for this purpose.

4. Use environment variables.

   Setting environment variables is useful if you're doing development
   work on a machine other than an |EC2| instance.

|IAM| Roles for Tasks
---------------------

If your application uses an |ECS| task definition or :code:`RunTask` operation,
use :ecs-dg:`IAM Roles for Tasks <task-iam-roles>`
to specify an IAM role that can be used by the containers in a task.

|IAM| Roles for |EC2| Instances
-------------------------------

If you are running your application on an |EC2| instance, 
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

.. toctree::
   :titlesonly:
   :maxdepth: 1

   SDK Metrics <sdk-metrics>
   Custom HTTP Client <custom-http>
