.. Copyright 2010-2016 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.


########
Sessions
########

Use sessions to define configurations for service clients.

In the AWS SDK for Go, a session is an object that contains
configuration information for :doc:`service clients <making-requests>`,
which you use to interact with AWS services. For example, sessions can
include information about the region where requests will be sent, which
credentials to use, or additional request handlers. Whenever you create
a service client, you must specify a session. For more information about
sessions, see the :sdk-go-api-deep:`session <aws/session.html>` 
package in the |sdk-go-api|.

Sessions can be shared across all service clients that share the same
base configuration. The Session is built from the SDK's default
configuration and request handlers.

Sessions should be cached when possible, because creating a new Session
will load all configuration values from the environment, and config
files each time the Session is created. Sharing the Session value across
all of your service clients will ensure the configuration is loaded the
fewest number of times possible.

.. _concurrency:

Concurrency
===========

Sessions are safe to use concurrently as long as the Session is not
being modified. The SDK will not modify the Session once the Session has
been created. Creating service clients concurrently from a shared
Session is safe.

.. _sessions-with-shared-config:

Sessions with Shared Config
===========================

Sessions can be created using the method above that will only load the
additional config if the ``AWS_SDK_LOAD_CONFIG`` environment variable is
set. Alternatively you can explicitly create a Session with shared
config enabled. To do this you can use ``NewSessionWithOptions`` to
configure how the Session will be created. Using the
``NewSessionWithOptions`` with ``SharedConfigState`` set to
``SharedConfigEnabled`` will create the session as if the
``AWS_SDK_LOAD_CONFIG`` environment variable was set.

.. _creating-sessions:

Creating Sessions
=================

When creating ``Session`` optional ``aws.Config`` values can be passed
in that will override the default, or loaded config values the Session
is being created with. This allows you to provide additional, or case
based, configuration as needed.

By default ``NewSession`` will only load credentials from the shared
credentials file (:file:`~/.aws/credentials`). If the ``AWS_SDK_LOAD_CONFIG``
environment variable is set to a truthy value the Session will be
created from the configuration values from the shared config
(:file:`~/.aws/config`) and shared credentials (:file:`~/.aws/credentials`) files. 
See the section Sessions from Shared Config for more information.

Create a Session with the default config and request handlers. With
credentials region, and profile loaded from the environment and shared
config automatically. Requires the ``AWS_PROFILE`` to be set, or
``default`` is used.

.. code:: go

    sess, err := session.NewSession()

The SDK provides a :sdk-go-api-deep:`default configuration <aws/defaults/`>, 
which is used by all sessions unless you override a field. For example, 
you can specify an AWS region when you create a session by using the 
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
    
Create Session With Option Overrides
====================================

In addition to ``NewSession``, Sessions can be created using
``NewSessionWithOptions``. This func allows you to control and override
how the Session will be created through code instead of being driven by
environment variables only.

Use :sdk-go-api-deep:`NewSessionWithOptions <aws/session/#NewSessionWithOptions>` 
when you want to provide the config profile, or override the shared config state 
(AWS\_SDK\_LOAD\_CONFIG).

.. code:: go

    // Equivalent to session.New
    sess, err := session.NewSessionWithOptions(session.Optons{})

    // Specify profile to load for the session's config
    sess, err := session.NewSessionWithOptions(session.Optons{
         Profile: "profile_name",
    })

    // Specify profile for config and region for requests
    sess, err := session.NewSessionWithOptions(session.Options{
         Config: aws.Config{Region: aws.String("us-east-1")},
         Profile: "profile_name",
    })

    // Force enable Shared Config support
    sess, err := session.NewSessionWithOptions(session.Optons{
        SharedConfigState: SharedConfigEnable,
    })

Deprecated ``New``
------------------

The ``New`` function has been deprecated because it does not provide
good way to return errors that occur when loading the configuration
files and values. Because of this, ``NewSession`` was created so errors
can be retrieved when creating a session fails.

Shared Config Fields
--------------------

By default the SDK will only load the shared credentials file's
(:file:`~/.aws/credentials`) credentials values, and all other config is
provided by the environment variables, SDK defaults, and user provided
aws.Config values.

If the ``AWS_SDK_LOAD_CONFIG`` environment variable is set, or
SharedConfigLoadEnable option is used to create the Session the full
shared config values will be loaded. This includes credentials, region,
and support for assume role. In addition the Session will load its
configuration from both the shared config file (:file:`~/.aws/config`) and
shared credentials file (:file:`~/.aws/credentials`). Both files have the same
format.

If both config files are present the configuration from both files will
be read. The Session will be created from configuration values from the
shared credentials file (:file:`~/.aws/credentials`) over those in the shared
credentials file (:file:`~/.aws/config`).

See the :sdk-go-api-deep:`session package's documentation <aws/session/>`
for more information on shared config setup.

.. _environment-variables:

Environment Variables
=====================

When a Session is created several environment variables can be set to
adjust how the SDK functions, and what configuration data it loads when
creating Sessions. All environment values are optional, but some values
like credentials require multiple of the values to set or the partial
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
service client:

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

You can use the :sdk-go-api-deep:`Copy <aws/session/#SessionCopy>` method to create 
copies of sessions. Copying sessions is useful when you want to create multiple 
sessions that have similar settings. Each time you copy a session, you can specify
different values for any field. For example, the following snippet
copies the ``sess`` session while overriding the ``Region`` field to
``us-east-1``:

.. code:: go

    usEast1Sess := sess.Copy(&aws.Config{Region: aws.String("us-east-1")})