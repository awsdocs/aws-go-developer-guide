.. Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _custom-http-client:
                       
#############################
Creating a Custom HTTP Client
#############################

.. meta::
   :description: Create a custom HTTP client with the |sdk-go| to specify custom timeout values.
   :keywords: HTTP, timeout

The |sdk-go| uses a default HTTP client with default configuration values.
Although you can change some of these configuration values,
the default HTTP client and transport are not sufficiently configurable for customers
using the |sdk-go| in an environment with high throughput and low latency requirements.
This section describes how to create a custom HTTP client,
and use that client to create |sdk-go| calls.

To assist you in creating a custom HTTP client,
this section describes how to create a structure to encapsulate the custom settings,
create a function to create a custom HTTP client based on those settings,
and use that custom HTTP client to call an |sdk-go| service client.

Let's define what we want to customize.

Dialer.KeepAlive
================

This setting represents the keep-alive period for an active network connection.

Set to a negative value to disable keep-alives.

Set to **0** to enable keep-alives if supported by the protocol and operating system.

Network protocols or operating systems that do not support keep-alives ignore this field.
By default, TCP enables keep alive.

See https://golang.org/pkg/net/#Dialer.KeepAlive

We'll call this ``ConnKeepAlive`` as **time.Duration**.

.. _timeout-struct-connect:

Dialer.Timeout
==============

This setting represents the maximum amount of time a dial to wait for a connection to be created.

Default is 30 seconds.

See https://golang.org/pkg/net/#Dialer.Timeout

We'll call this ``Connect`` as **time.Duration**.

.. _timeout-struct-expect-continue:

Transport.ExpectContinueTimeout
===============================

This setting represents the maximum amount of time to wait for a server's first response headers
after fully writing the request headers,
if the request has an "Expect: 100-continue" header.
This time does not include the time to send the request header.
The HTTP client sends its payload after this timeout is exhausted.

Default 1 second.

Set to **0** for no timeout and send request payload without waiting.
One use case is when you run into issues with proxies or third party services that take a session
similar to the use of |S3| in the function shown later.

See https://golang.org/pkg/net/http/#Transport.ExpectContinueTimeout

We'll call this ``ExpectContinue`` as **time.Duration**.

.. _timeout-struct-idle-conn-timeout:

Transport.IdleConnTimeout
=========================

This setting represents the maximum amount of time to keep an idle network connection alive between HTTP requests.

Set to **0** for no limit.

See https://golang.org/pkg/net/http/#Transport.IdleConnTimeout

We'll call this ``IdleConn`` as **time.Duration**.

.. _timeout-struct-keep-alive:

.. _timeout-struct-max-idle-conns:

Transport.MaxIdleConns
======================

This setting represents the maximum number of idle (keep-alive) connections across all hosts.
One use case for increasing this value is when you are seeing many connections in a short period from the same clients

**0** means no limit.

See https://golang.org/pkg/net/http/#Transport.MaxIdleConns

We'll call this ``MaxAllIdleConns`` as **int**.

.. _timeout-struct-max-idle-conn-per-host:

Transport.MaxIdleConnsPerHost
=============================

This setting represents the maximum number of idle (keep-alive) connections to keep per-host.
One use case for increasing this value is when you are seeing many connections in a short period from the same clients

Default is two idle connections per host.

Set to **0** to use DefaultMaxIdleConnsPerHost (2).

See https://golang.org/pkg/net/http/#Transport.MaxIdleConnsPerHost

We'll call this ``MaxHostIdleConns`` as **int**.  

.. _timeout-struct-response-header-timeout:

Transport.ResponseHeaderTimeout
===============================

This setting represents the maximum amount of time to wait for a client to read the response header.

If the client isn't able to read the response's header within this duration,
the request fails with a timeout error.

Be careful setting this value when using long-running Lambda functions,
as the operation does not return any response headers until the Lambda function has finished or timed out.
However, you can still use this option with the **InvokeAsync** API operation.

Default is no timeout; wait forever.

See https://golang.org/pkg/net/http/#Transport.ResponseHeaderTimeout

We'll call this ``ResponseHeader`` as **time.Duration**.

.. _timeout-struct-tls-handshake-timeout:

Transport.TLSHandshakeTimeout
=============================

This setting represents the maximum amount of time waiting for a TLS handshake to be completed.

Default is 10 seconds.

Zero means no timeout.

See https://golang.org/pkg/net/http/#Transport.TLSHandshakeTimeout

We'll call this ``TLSHandshake`` as **time.Duration**.

.. _set_imports:

Create Import Statement
=======================

The complete example imports the following Go packages.

.. literalinclude:: s3.go.customHttpClient.import.txt
   :dedent: 0
   :language: go

.. _timeout-struct:

Creating a Timeout Struct
=========================

Let's create a struct to hold the timeout values we want to be able to set
on our HTTP client.

.. literalinclude:: s3.go.customHttpClient_struct.txt
   :dedent: 0
   :language: go

.. _timeout-func:

Creating a Function to Create a Custom HTTP Client
==================================================

Next let's create a function that takes a **ClientTimeout** struct
and creates a custom HTTP client based on those timeout values.

.. literalinclude:: s3.go.customHttpClient_client.txt
   :dedent: 0
   :language: go

.. _s3-client:

Using a Custom HTTP Client
==========================

Let's create a custom HTTP client and use it to 
create an |S3| client.

The following example creates an **http.Client** that is configured to have:

- a five second TCP connection timeout
- a five second TLS handshake timeout
- a five second wait for the HTTP response headers

.. literalinclude:: s3.go.customHttpClient_session.txt
   :dedent: 8
   :language: go

All of these settings give the client approximately 15 seconds create a connection,
do a TLS handshake, and receive the response headers from the service.
The time that the client takes to read the response body is not covered by these timeouts.
To specify a total timeout for the request to include reading the response body,
use the |sdk-go| client's **WithContext** API operation methods,
such as the |S3| operation
`PutObjectWithContext
<https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutObjectWithContext>`_
with a **context.Withtimeout**.

The following example uses a timeout context to limit the total time an API request can be active to a maximum of 20 seconds.
The SDK must be able to read the full HTTP response body (Object body) within the timeout or the SDK returns a timeout error.
For API operations that return an **io.ReadCloser** in their response type,
the Context's timeout includes reading the content from the **io.ReadCloser**.

.. literalinclude:: s3.go.customHttpClient.get_object.txt
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/s3/CustomClient/CustomHttpClient.go>`_
on GitHub.
