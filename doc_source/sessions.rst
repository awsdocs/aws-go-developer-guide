.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.


###########################################################
Using Sessions to Configure Service Clients in the |sdk-go|
###########################################################

.. meta::
   :description: Use sessions to define configurations for service clients.
   :keywords: service client configuration

In the |sdk-go|, a session is an object that contains
configuration information for :doc:`service clients <making-requests>`,
which you use to interact with AWS services. For example, sessions can
include information about the region where requests will be sent, which
credentials to use, or additional request handlers. Whenever you create
a service client, you must specify a session. For more information about
sessions, see the :sdk-go-api-deep:`session <aws/session/>`
package in the |sdk-go-api|.

Sessions can be shared across all service clients that share the same
base configuration. The session is built from the SDK's default
configuration and request handlers.

You should cache sessions when possible. This is because creating a new session
loads all configuration values from the environment and config
files each time the session is created. Sharing the Session value across
all of your service clients ensures the configuration is loaded the
fewest number of times possible.


.. _concurrency:

Concurrency
===========

Sessions are safe to use concurrently as long as the session isn't
being modified. The SDK does not modify the session once the session is
created. Creating service clients concurrently from a shared
session is safe.

.. _sessions-with-shared-config:

Sessions with Shared Config
===========================

Using the previous method, you can create sessions that
load the additional config only if the ``AWS_SDK_LOAD_CONFIG`` environment variable is
set. Alternatively you can explicitly create a session with a shared
config enabled. To do this, you can use ``NewSessionWithOptions`` to
configure how the session is created. Using the
``NewSessionWithOptions`` with ``SharedConfigState`` set to
``SharedConfigEnabled`` will create the session as if the
``AWS_SDK_LOAD_CONFIG`` environment variable was set.

.. _creating-sessions:

Creating Sessions
=================

When you create a ``session``, you can pass in optional ``aws.Config`` values
that will override the default, or loaded config values the session
is being created with. This allows you to provide additional, or case-
based, configuration as needed.

By default ``NewSession`` will only load credentials from the shared
credentials file (:file:`~/.aws/credentials`). If the ``AWS_SDK_LOAD_CONFIG``
environment variable is set to a truthy value, the session is
created from the configuration values from the shared config
(:file:`~/.aws/config`) and shared credentials (:file:`~/.aws/credentials`) files.
See the section Sessions from Shared Config for more information.

Create a session with the default config and request handlers. With
credentials region, and profile loaded from the environment and shared
config automatically. Requires the ``AWS_PROFILE`` to be set, or
``default`` is used.

.. code:: go

    sess, err := session.NewSession()

The SDK provides a :sdk-go-api-deep:`default configuration <aws/defaults/>`,
which all sessions use unless you override a field. For example,
you can specify an AWS Region when you create a session by using the
``aws.Config`` struct. For more information about the fields you can
specify, see the :sdk-go-api-deep:`aws.Config <aws/#Config>`
in the |sdk-go-api|.

.. code:: go

    sess, err := session.NewSession(&aws.Config{
        Region: aws.String("us-east-1")},
    )

Create an |S3| client instance from a session:

.. code:: go

    sess, err := session.NewSession()
    if err != nil {
        // Handle Session creation error
    }
    svc := s3.New(sess)

.. _create-session-with-option-overrides:

Create Session with Option Overrides
====================================

In addition to ``NewSession``, you can create sessions using
``NewSessionWithOptions``. This func allows you to control and override
how the session will be created through code instead of being driven by
environment variables only.

Use :sdk-go-api-deep:`NewSessionWithOptions <aws/session/#NewSessionWithOptions>`
when you want to provide the config profile, or override the shared config state
(AWS\_SDK\_LOAD\_CONFIG).

.. code:: go

    // Equivalent to session.New
    sess, err := session.NewSessionWithOptions(session.Options{})

    // Specify profile to load for the session's config
    sess, err := session.NewSessionWithOptions(session.Options{
         Profile: "profile_name",
    })

    // Specify profile for config and region for requests
    sess, err := session.NewSessionWithOptions(session.Options{
         Config: aws.Config{Region: aws.String("us-east-1")},
         Profile: "profile_name",
    })

    // Force enable Shared Config support
    sess, err := session.NewSessionWithOptions(session.Options{
        SharedConfigState: SharedConfigEnable,
    })

    // Assume an IAM role with MFA prompting for token code on stdin
    sess := session.Must(session.NewSessionWithOptions(session.Options{
        AssumeRoleTokenProvider: stscreds.StdinTokenProvider,
        SharedConfigState: SharedConfigEnable,
    }))


Deprecated ``New``
------------------

The ``New`` function has been deprecated because it doesn't provide
a good way to return errors that occur when loading the configuration
files and values. Because of this, ``NewSession`` was created so errors
can be retrieved when creating a session fails.

Shared Config Fields
--------------------

By default, the SDK only loads the shared credentials file's
(:file:`~/.aws/credentials`) credentials values. All other config is
provided by the environment variables, SDK defaults, and user-provided
aws.Config values.

If the ``AWS_SDK_LOAD_CONFIG`` environment variable is set, or
the SharedConfigLoadEnable option is used to create the session, the full
shared config values will be loaded. This includes credentials, region,
and support for assumed role. In addition, the session will load its
configuration from both the shared config file (:file:`~/.aws/config`) and
shared credentials file (:file:`~/.aws/credentials`). Both files have the same
format.

If both config files are present, the configuration from both files is
read. The session is created from configuration values from the
shared credentials file (:file:`~/.aws/credentials`) instead of those in the shared
credentials file (:file:`~/.aws/config`).

See the :sdk-go-api-deep:`session package's documentation <aws/session/>`
for more information on shared config setup.

.. _environment-variables:

Environment Variables
=====================

When a session is created, you can set several environment variables to
adjust how the SDK functions, and what configuration data it loads when
creating sessions. All environment values are optional. However, some values
such as credentials require multiple of the values to set or the partial
values will be ignored. All environment variable values are strings
unless otherwise noted.

See the :sdk-go-api-deep:`session package's documentation <aws/session/>`
for more information on environment variable setup.

.. _adding-request-handlers:

Adding Request Handlers
=======================

You can add handlers to a session for processing HTTP requests. All
service clients that use the session inherit the handlers. For example,
the following handler logs every request and its payload made by a
service client.

.. code:: go

    // Create a session, and add additional handlers for all service
    // clients created with the Session to inherit. Adds logging handler.
    sess, err := session.NewSession()
    sess.Handlers.Send.PushFront(func(r *request.Request) {
        // Log every request made and its payload
        logger.Println("Request: %s/%s, Payload: %s",
            r.ClientInfo.ServiceName, r.Operation, r.Params)
    })

.. _copying-a-session:

Copying a Session
=================

You can use the :sdk-go-api-deep:`Copy <aws/session/#Session.Copy>` method to create
copies of sessions. Copying sessions is useful when you want to create multiple
sessions that have similar settings. Each time you copy a session, you can specify
different values for any field. For example, the following snippet
copies the ``sess`` session while overriding the ``Region`` field to
``us-east-1``:

.. code:: go

    usEast1Sess := sess.Copy(&aws.Config{Region: aws.String("us-east-1")})
