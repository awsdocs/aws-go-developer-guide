.. Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

###################################
Setting the TLS Version in |sdk-go|
###################################

.. meta::
   :description: Describes how to set the TLS version for the |sdk-go|.

The Issue
=========

All AWS APIs use a secure protocol to ensure that communication between your application and AWS servers is encrypted.
To ensure this communication cannot be compromised,
AWS will soon require that you use TLS (transport layer security) version 1.2 or greater.
Support for previous versions of TLS will end on March 31, 2020.

How do I get my TLS Version?
============================

Go has supported TLS 1.2 since the 1.13 release.
To see your Go version, enter the following command.

:code:`go version`

How do I set my TLS Version?
============================

You can set the TLS version to 1.2 using the following code.

1. Create a custom HTTP transport to require a minimum TLS 1.2 version.

   .. literalinclude:: s3.go.set_tls_12_transport.txt
   :dedent: 4
   :language: go

2. Configure the transport.

   In Go versions before 1.13:

   .. literalinclude:: s3.go.set_tls_12_cfg_112.txt
   :dedent: 8
   :language: go

   In Go versions from 1.13 on:

   .. literalinclude:: s3.go.set_tls_12_cfg_113.txt
   :dedent: 8
   :language: go

3. Create an HTTP client with the configured transport, and use that to create an SDK client,
   in this case for Amazon S3.

   .. literalinclude:: s3.go.set_tls_12_client.txt
   :dedent: 4
   :language: go   

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/s3/s3SetTls12.go>`_
on GitHub.
