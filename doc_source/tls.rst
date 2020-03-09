.. Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#####################################
Enforcing TLS Version 1.2 in |sdk-go|
#####################################

.. meta::
   :description: Describes how to set the TLS version for the |sdk-go|.

To add increased security when communicating with AWS services, you should configure your client to use TLS 1.2 or later.
TLS 1.2 has been the default in Go since release 1.13 in September 2019.

How do I set my TLS Version?
============================

You can set the TLS version to 1.2 using the following code.

1. Create a custom HTTP transport to require a minimum TLS 1.2 version.

   .. code:: go
             
       tr := &http.Transport{
           TLSClientConfig: &tls.Config{
               MinVersion: tls.VersionTLS12,
           },
       }

2. Configure the transport.

   .. code:: go
             
       // In Go versions before 1.13
       err := http2.ConfigureTransport(tr)
       if err != nil {
           fmt.Println("Got an error configuring HTTP transport")
           fmt.Println(err)
           return
       }

       // In Go versions from 1.13 on:
       tr.ForceAttemptHTTP2 = true

3. Create an HTTP client with the configured transport, and use that to create a session.
    REGION is the AWS Region, such as `us-west-2`.

   .. code:: go
       client := http.Client{Transport: tr}

       sess := session.Must(session.NewSession(&aws.Config{
           Region:     &REGION,
           HTTPClient: &client,
       }))

4. Confirm your TLS version with the following Go code.

   .. code:: go
             
       version := "Unknown"
   
       switch tr.TLSClientConfig.MinVersion {
       case tls.VersionTLS10:
           version = "TLS 1.0"
       case tls.VersionTLS11:
           version = "TLS 1.1"
       case tls.VersionTLS12:
           version = "TLS 1.2"
       case tls.VersionTLS13:
           version = "TLS 1.3"
       }

       fmt.Println("TLS version: " + version)
