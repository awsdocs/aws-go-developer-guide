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

How do I set my TLS version?
============================

You can set the TLS version to 1.2 using the following code.

1. Create a custom HTTP transport to require a minimum version of TLS 1.2

   .. code:: go
             
       tr := &http.Transport{
           TLSClientConfig: &tls.Config{
               MinVersion: tls.VersionTLS12,
           },
       }

2. Configure the transport.

   .. code:: go
             
       // In Go versions earlier than 1.13
       err := http2.ConfigureTransport(tr)
       if err != nil {
           fmt.Println("Got an error configuring HTTP transport")
           fmt.Println(err)
           return
       }

       // In Go versions later than 1.13
       tr.ForceAttemptHTTP2 = true

3. Create an HTTP client with the configured transport, and use that to create a session.
   REGION is the AWS Region, such as `us-west-2`.

   .. code:: go

       client := http.Client{Transport: tr}

       sess := session.Must(session.NewSession(&aws.Config{
           Region:     &REGION,
           HTTPClient: &client,
       }))

4. Use the following function to confirm your TLS version.

   .. code:: go
             
       func GetTLSVersion(tr *http.Transport) string {
           switch tr.TLSClientConfig.MinVersion {
           case tls.VersionTLS10:
               return "TLS 1.0"
           case tls.VersionTLS11:
               return "TLS 1.1"
           case tls.VersionTLS12:
               return "TLS 1.2"
           case tls.VersionTLS13:
               return "TLS 1.3"
           }

           return "Unknown"
       }

5. Confirm your TLS version by calling `GetTLSVersion`.

   .. code:: go
             
       if tr, ok := s3Client.Config.HTTPClient.Transport.(*http.Transport); ok {
           log.Printf("Client uses %v", GetTLSVersion(tr))
       }
