.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-iam-certificates:

######################################
Working with |IAM| Server Certificates
######################################

.. meta::
   :description: Learn to manage |IAM| server certificates using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, IAM server certificates

This Go example shows you how to carry out basic tasks for managing server certificate HTTPS connections with the |sdk-go|.

You can download complete versions of these example files from the :doc-examples-go:`aws-doc-sdk-examples <s3>` repository on GitHub.

.. _iam-certificates-scenario:

Scenario
========

To enable HTTPS connections to your website or application on AWS, you need an SSL/TLS server certificate. To use a certificate that you obtained from an external provider with your website or application on AWS, you must upload the certificate to |IAM| or import it into AWS Certificate Manager.

In this example, you use a series of Go routines to manage policies in |IAM|. The routines use the |sdk-go| |IAM| client methods that follow:

* :sdk-go-api-deep:`ListServerCertificates <service/iam/#IAM.ListServerCertificates>`
* :sdk-go-api-deep:`GetServerCertificate <service/iam/#IAM.GetServerCertificate>`
* :sdk-go-api-deep:`UpdateServerCertificate <service/iam/#IAM.UpdateServerCertificate>`
* :sdk-go-api-deep:`DeleteServerCertificate <service/iam/#IAM.DeleteServerCertificate>`

.. _iam-certificates-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>` and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with server certificates. To learn more, see :iam-ug:`Working with Server Certificates <id_credentials_server-certs>` in the |IAM-ug|.

.. _iam-example-list-certificates:

List Your Server Certificates
=============================

This code lists your certificates.

Create a new Go file named :file:`iam_listservercerts.go`. You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_listservercerts.go
   :lines: 26,29-34

Call ``ListServerCertificates`` and print the details.

.. literalinclude:: example_code/iam/iam_listservercerts.go
   :lines: 36-49


.. _iam-example-get-certificate:

Get a Server Certificate
========================

In this example, you retrieve an existing server certificate.

Create a new Go file named :file:`iam_getservercert.go`. You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_getservercert.go
   :lines: 15-23

Set up a new IAM client.

.. literalinclude:: example_code/iam/iam_getservercert.go
   :lines: 27,30-35

Call ``GetServerCertificate``, passing the name of the certificate, and print the results.

.. literalinclude:: example_code/iam/iam_getservercert.go
   :lines: 37-46

.. _iam-example-update-certificate:

Update a Server Certificate
===========================

In this example, you update an existing server certificate.

Create a new Go file named :file:`iam_updateservercert.go`. You call the ``UpdateServerCertificate`` method of the |IAM| service object to change the name of the certificate.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_updateservercert.go
   :lines: 15-23

Set up a new |IAM| client.

.. literalinclude:: example_code/iam/iam_updateservercert.go
   :lines: 27,30-35

Update the certificate name.

.. literalinclude:: example_code/iam/iam_updateservercert.go
   :lines: 37-47

.. _iam_example_delete_server_certificate:

Delete a Server Certificate
===========================

In this example, you delete an existing server certificate.

Create a new Go file named :file:`iam_deleteservercert.go`. You call the ``DeleteServerCertificate`` method of the |IAM| service object to change the name of the certificate.

You must import the relevant Go and |sdk-go| packages by adding the following lines.

.. literalinclude:: example_code/iam/iam_deleteservercert.go
   :lines: 15-23

Set up a new |IAM| client.

.. literalinclude:: example_code/iam/iam_deleteservercert.go
   :lines: 27,30-35

Call the method to delete the certificate, specifying the name of certificate.

.. literalinclude:: example_code/iam/iam_deleteservercert.go
   :lines: 37-46


