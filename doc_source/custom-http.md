# Creating a Custom HTTP Client<a name="custom-http"></a>

The AWS SDK for Go uses a default HTTP client with default configuration values\. Although you can change some of these configuration values, the default HTTP client and transport are not sufficiently configurable for customers using the AWS SDK for Go in an environment with high throughput and low latency requirements\. This section describes how to create a custom HTTP client, and use that client to create AWS SDK for Go calls\.

To assist you in creating a custom HTTP client, this section describes how to create a structure to encapsulate the custom settings, create a function to create a custom HTTP client based on those settings, and use that custom HTTP client to call an AWS SDK for Go service client\.

Let’s define what we want to customize\.

## Dialer\.KeepAlive<a name="dialer-keepalive"></a>

This setting represents the keep\-alive period for an active network connection\.

Set to a negative value to disable keep\-alives\.

Set to **0** to enable keep\-alives if supported by the protocol and operating system\.

Network protocols or operating systems that do not support keep\-alives ignore this field\. By default, TCP enables keep alive\.

See [https://golang\.org/pkg/net/\#Dialer\.KeepAlive](https://golang.org/pkg/net/#Dialer.KeepAlive) 

We’ll call this `ConnKeepAlive` as **time\.Duration**\.

## Dialer\.Timeout<a name="timeout-struct-connect"></a>

This setting represents the maximum amount of time a dial to wait for a connection to be created\.

Default is 30 seconds\.

See [https://golang\.org/pkg/net/\#Dialer\.Timeout](https://golang.org/pkg/net/#Dialer.Timeout) 

We’ll call this `Connect` as **time\.Duration**\.

## Transport\.ExpectContinueTimeout<a name="timeout-struct-expect-continue"></a>

This setting represents the maximum amount of time to wait for a server’s first response headers after fully writing the request headers, if the request has an “Expect: 100\-continue” header\. This time does not include the time to send the request header\. The HTTP client sends its payload after this timeout is exhausted\.

Default 1 second\.

Set to **0** for no timeout and send request payload without waiting\. One use case is when you run into issues with proxies or third party services that take a session similar to the use of Amazon S3 in the function shown later\.

See [https://golang\.org/pkg/net/http/\#Transport\.ExpectContinueTimeout](https://golang.org/pkg/net/http/#Transport.ExpectContinueTimeout) 

We’ll call this `ExpectContinue` as **time\.Duration**\.

## Transport\.IdleConnTimeout<a name="timeout-struct-idle-conn-timeout"></a>

This setting represents the maximum amount of time to keep an idle network connection alive between HTTP requests\.

Set to **0** for no limit\.

See [https://golang\.org/pkg/net/http/\#Transport\.IdleConnTimeout](https://golang.org/pkg/net/http/#Transport.IdleConnTimeout) 

We’ll call this `IdleConn` as **time\.Duration**\.

## Transport\.MaxIdleConns<a name="timeout-struct-max-idle-conns"></a>

This setting represents the maximum number of idle \(keep\-alive\) connections across all hosts\. One use case for increasing this value is when you are seeing many connections in a short period from the same clients

 **0** means no limit\.

See [https://golang\.org/pkg/net/http/\#Transport\.MaxIdleConns](https://golang.org/pkg/net/http/#Transport.MaxIdleConns) 

We’ll call this `MaxAllIdleConns` as **int**\.

## Transport\.MaxIdleConnsPerHost<a name="timeout-struct-max-idle-conn-per-host"></a>

This setting represents the maximum number of idle \(keep\-alive\) connections to keep per\-host\. One use case for increasing this value is when you are seeing many connections in a short period from the same clients

Default is two idle connections per host\.

Set to **0** to use DefaultMaxIdleConnsPerHost \(2\)\.

See [https://golang\.org/pkg/net/http/\#Transport\.MaxIdleConnsPerHost](https://golang.org/pkg/net/http/#Transport.MaxIdleConnsPerHost) 

We’ll call this `MaxHostIdleConns` as **int**\.

## Transport\.ResponseHeaderTimeout<a name="timeout-struct-response-header-timeout"></a>

This setting represents the maximum amount of time to wait for a client to read the response header\.

If the client isn’t able to read the response’s header within this duration, the request fails with a timeout error\.

Be careful setting this value when using long\-running Lambda functions, as the operation does not return any response headers until the Lambda function has finished or timed out\. However, you can still use this option with the **InvokeAsync** API operation\.

Default is no timeout; wait forever\.

See [https://golang\.org/pkg/net/http/\#Transport\.ResponseHeaderTimeout](https://golang.org/pkg/net/http/#Transport.ResponseHeaderTimeout) 

We’ll call this `ResponseHeader` as **time\.Duration**\.

## Transport\.TLSHandshakeTimeout<a name="timeout-struct-tls-handshake-timeout"></a>

This setting represents the maximum amount of time waiting for a TLS handshake to be completed\.

Default is 10 seconds\.

Zero means no timeout\.

See [https://golang\.org/pkg/net/http/\#Transport\.TLSHandshakeTimeout](https://golang.org/pkg/net/http/#Transport.TLSHandshakeTimeout) 

We’ll call this `TLSHandshake` as **time\.Duration**\.

## Create Import Statement<a name="set-imports"></a>

The complete example imports the following Go packages\.

```
import (
    "bytes"
    "context"
    "flag"
    "fmt"
    "io"
    "net"
    "net/http"
    "time"

    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"

    "golang.org/x/net/http2"
)
```

## Creating a Timeout Struct<a name="timeout-struct"></a>

Let’s create a struct to hold the timeout values we want to be able to set on our HTTP client\.

```
type HTTPClientSettings struct {
    Connect          time.Duration
    ConnKeepAlive    time.Duration
    ExpectContinue   time.Duration
    IdleConn         time.Duration
    MaxAllIdleConns  int
    MaxHostIdleConns int
    ResponseHeader   time.Duration
    TLSHandshake     time.Duration
}
```

## Creating a Function to Create a Custom HTTP Client<a name="timeout-func"></a>

Next let’s create a function that takes a **ClientTimeout** struct and creates a custom HTTP client based on those timeout values\.

```
func NewHTTPClientWithSettings(httpSettings HTTPClientSettings) (*http.Client, error) {
    var client http.Client
    tr := &http.Transport{
        ResponseHeaderTimeout: httpSettings.ResponseHeader,
        Proxy:                 http.ProxyFromEnvironment,
        DialContext: (&net.Dialer{
            KeepAlive: httpSettings.ConnKeepAlive,
            DualStack: true,
            Timeout:   httpSettings.Connect,
        }).DialContext,
        MaxIdleConns:          httpSettings.MaxAllIdleConns,
        IdleConnTimeout:       httpSettings.IdleConn,
        TLSHandshakeTimeout:   httpSettings.TLSHandshake,
        MaxIdleConnsPerHost:   httpSettings.MaxHostIdleConns,
        ExpectContinueTimeout: httpSettings.ExpectContinue,
    }

    // So client makes HTTP/2 requests
    err := http2.ConfigureTransport(tr)
    if err != nil {
        return &client, err
    }

    return &http.Client{
        Transport: tr,
    }, nil
}
```

## Using a Custom HTTP Client<a name="s3-client"></a>

Let’s create a custom HTTP client and use it to create an Amazon S3 client\.

The following example creates an **http\.Client** that is configured to have:
+ a five second TCP connection timeout
+ a five second TLS handshake timeout
+ a five second wait for the HTTP response headers

```
httpClient, err := NewHTTPClientWithSettings(HTTPClientSettings{
    Connect:          5 * time.Second,
    ExpectContinue:   1 * time.Second,
    IdleConn:         90 * time.Second,
    ConnKeepAlive:    30 * time.Second,
    MaxAllIdleConns:  100,
    MaxHostIdleConns: 10,
    ResponseHeader:   5 * time.Second,
    TLSHandshake:     5 * time.Second,
})
if err != nil {
    fmt.Println("Got an error creating custom HTTP client:")
    fmt.Println(err)
    return
}

sess := session.Must(session.NewSession(&aws.Config{
    HTTPClient: httpClient,
}))

svc := s3.New(sess)
```

All of these settings give the client approximately 15 seconds create a connection, do a TLS handshake, and receive the response headers from the service\. The time that the client takes to read the response body is not covered by these timeouts\. To specify a total timeout for the request to include reading the response body, use the AWS SDK for Go client’s **WithContext** API operation methods, such as the Amazon S3 operation [PutObjectWithContext](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutObjectWithContext) with a **context\.Withtimeout**\.

The following example uses a timeout context to limit the total time an API request can be active to a maximum of 20 seconds\. The SDK must be able to read the full HTTP response body \(Object body\) within the timeout or the SDK returns a timeout error\. For API operations that return an **io\.ReadCloser** in their response type, the Context’s timeout includes reading the content from the **io\.ReadCloser**\.

```
ctx, cancelFn := context.WithTimeout(context.TODO(), 20*time.Second)
defer cancelFn()

resp, err := svc.GetObjectWithContext(ctx, &s3.GetObjectInput{
    Bucket: bucket,
    Key:    object,
})
if err != nil {
    return body, err
}

return resp.Body, nil
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/s3/CustomClient/CustomHttpClient.go) on GitHub\.