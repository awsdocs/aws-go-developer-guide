.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

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

The |sdk-go| uses a default HTTP client with default configuration values,
such as
:sdk-go-api-deep:`MaxRetries <aws/#Config.WithMaxRetries>`.
Although you can change some of these configuration values,
the default HTTP client and transport are not sufficiently configurable for customers
using the |sdk-go| in an environment with high throughput and low latency requirements.
This section describes how to create a custom HTTP client,
and use that client to create |sdk-go| calls.

To assist you in creating a custom HTTP client,
this section describes how to create a structure to encapsulate the custom settings,
create a function to create a custom HTTP client based on those settings,
and use that custom HTTP client to call an |sdk-go| service client.

We want to be able to customize the following settings.

.. _timeout-struct-connect:

Dialer.Timeout
--------------

This setting represents the maximum amount of time a dial to wait for a connection to complete.

Default is no timeout.

See https://golang.org/pkg/net/#Dialer.Timeout

We'll call this ``Connect`` as **time.Duration**.

.. _timeout-struct-expect-continue:

Transport.ExpectContinueTimeout
-------------------------------

This setting represents the maximum amount of time to wait for a server's first response headers
after fully writing the request headers,
if the request has an "Expect: 100-continue" header.
This time does not include the time to send the request header.
The HTTP client sends its payload after this timeout is exhausted.

Default 1 second.

Set to **0** for no timeout and send request payload without waiting.

See https://golang.org/pkg/net/http/#Transport.ExpectContinueTimeout

We'll call this ``ExpectContinue`` as **time.Duration**.

.. _timeout-struct-idle-conn-timeout:

Transport.IdleConnTimeout
-------------------------

This setting represents the maximum amount of time to keep an idle connection alive.

Set to **0** for no limit.

See https://golang.org/pkg/net/http/#Transport.IdleConnTimeout

We'll call this ``IdleConn`` as **time.Duration**.

.. _timeout-struct-keep-alive:

Dialer.KeepAlive
----------------

This setting represents the keep-alive period for an active network connection.

Set to a negative value to disable keep-alives.

Set to **0** to enable keep-alives if supported by the protocol and operating system.

Network protocols or operating systems that do not support keep-alives ignore this field.

See https://golang.org/pkg/net/#Dialer.KeepAlive

We'll call this ``KeepAlive`` as **time.Duration**.
  

.. _timeout-struct-max-idle-conns:

Transport.MaxIdleConns
----------------------

This setting represents the maximum number of idle (keep-alive) connections across all hosts.

**0** means no limit.

See https://golang.org/pkg/net/http/#Transport.MaxIdleConns

We'll call this ``MaxAllIdleConns`` as **int**.

.. _timeout-struct-max-idle-conn-per-host:

Transport.MaxIdleConnsPerHost
-----------------------------

This setting represents the maximum number of idle (keep-alive) connections to keep per-host.

Default is two idle connections per host.

Set to **0** to use DefaultMaxIdleConnsPerHost.

See https://golang.org/pkg/net/http/#Transport.MaxIdleConnsPerHost

We'll call this ``MaxHostIdleConns`` as **int**.  

.. _timeout-struct-response-header-timeout:

Transport.ResponseHeaderTimeout
-------------------------------

This setting represents the maximum amount of time to wait for a client to read the response header.

If the client isn't able to read the response's header within this duration,
the request fails with a timeout error.

Default is no timeout; wait forever.

See https://golang.org/pkg/net/http/#Transport.ResponseHeaderTimeout

We'll call this ``ResponseHeader`` as **time.Duration**.

.. _timeout-struct-tls-handshake-timeout:

Transport.TLSHandshakeTimeout
-----------------------------

This setting represents the maximum amount of time waiting for a TLS handshake to be completed.

Default is 10 seconds.

Zero means no timeout.

See https://golang.org/pkg/net/http/#Transport.TLSHandshakeTimeout

We'll call this ``TLSHandshake`` as **time.Duration**.

.. _timeout-struct:

Creating a Timeout Struct
=========================

Let's create a struct to hold the timeout values we want to be able to set
on our HTTP client.

.. code-block:: go

    // import "time"
    type HttpClientSettings struct {
        Connect          time.Duration
        ExpectContinue   time.Duration
        IdleConn         time.Duration
        KeepAlive        time.Duration
        MaxAllIdleConns  int
        MaxHostIdleConns int
        ResponseHeader   time.Duration
        TLSHandshake     time.Duration
    }

Next let's create a function that takes a **ClientTimeout** struct
and creates a custom HTTP client based on those timeout values.

.. code-block:: go

    // import (
    //     "net/http"
    //     "time"
    // )
    func NewHTTPClientWithTimeouts(httpSettings HttpClientSettings) *http.Client {
        return &http.Client{
            Transport: &http.Transport{
                ResponseHeaderTimeout: httpSettings.ResponseHeader,
                Proxy:                 http.ProxyFromEnvironment,
                DialContext:           (&net.Dialer{
                    KeepAlive: httpSettings.KeepAlive
                    DualStack: true,
                    Timeout:   httpSettings.Connect,
                }).DialContext,
                MaxIdleConns:          httpSettings.MaxAllIdleConns,
                IdleConnTimeout:       httpSettings.IdleConn,
                TLSHandshakeTimeout:   httpSettings.TLSHandshake,
                MaxIdleConnsPerHost:   httpSettings.MaxHostIdleConns,
                ExpectContinueTimeout: httpSettings.ExpectContinue,
            },
        }
    }

Let's create a function that use this function to create an &S3;
client with a custom HTTP client and access an item from an &S3; bucket.

.. code-block:: go

    // import "time"
    func ExampleS3WithCustomHTTPClient(bucket, key, region *string) io.ReadCloser {
        // Creating a SDK session using the SDK's default HTTP client,
        // http.DefaultClient.
        sess := session.Must(session.NewSession())

        // Create SDK S3 client with a HTTP client configured for custom timeouts.
        client := s3.New(sess, &aws.Config{
            Region:     region,
            HTTPClient: NewHTTPClientWithTimeouts(HttpClientSettings{
                Connect:            5 * time.Second,
                ExpectContinue:     1 * time.Second,
                IdleConn:          90 * time.Second,
                KeepAlive:         30 * time.Second,
                MaxAllIdleConns:  100,
                MaxHostIdleConns:  10,
                ResponseHeader:     5 * time.Second,
                TLSHandshake:       5 * time.Second,
            }),
        })

        obj, err := client.GetObject(&s3.GetObjectInput{
            Bucket: bucket,
            Key:    key,
        })
        if err != nil {
            fmt.Println("Got error calling GetObject in ExampleS3WithCustomHTTPClient:")
            fmt.Println(err.Error())
            os.Exit(1)
        }

        return obj.Body
    }

Finally, let's create another function that creates a session with a custome HTTP client
and an &S3; client using that session to again access an item from an &S3; bucket.

.. code-block:: go

    func exampleSharedClient(bucket, key, region *string) io.ReadCloser {
        // Create a shared SDK session to be used by all SDK clients.
        // All SDK clients share the HTTP client's timeout configuration.
        sess := session.Must(session.NewSession(&aws.Config{
            Region: region,
            HTTPClient: NewHTTPClientWithTimeouts(HttpClientSettings{
                Connect:          5 * time.Second,
                ExpectContinue:   1 * time.Second,
                IdleConn:         90 * time.Second,
                KeepAlive:        30 * time.Second,
                MaxAllIdleConns:  100,
                MaxHostIdleConns: 10,
                ResponseHeader:   5 * time.Second,
                TLSHandshake:     5 * time.Second,
            }),
        }))

        // Create an S3 SDK client with the shared SDK session,
        // which includes the same HTTP custom timeouts.
        client := s3.New(sess)

        // Make API operation calls with SDK clients, all sharing the same HTTP client timeout configuration.
        obj, err := client.GetObject(&s3.GetObjectInput{
            Bucket: bucket,
            Key:    key,
        })
        if err != nil {
            fmt.Println("Got error calling GetObject in exampleSharedClient:")
            fmt.Println(err.Error())
            os.Exit(1)
        }

        return obj.Body
    }

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/customHttpClient.go>`_
on GitHub.
