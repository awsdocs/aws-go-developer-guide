.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

##############################
AWS SDK for Go Developer Guide
##############################

.. meta::
   :description: Use the AWS SDK for Go to build Go applications that use AWS services.
   :keywords: AWS SDK for Go, code examples

Welcome to the |sdk-go|.
The |sdk-go| provides APIs and utilities that developers can use
to build Go applications that use AWS services, such as |EC2long| (|EC2|) and
|S3long| (|S3|).

The SDK removes the complexity of coding directly against a web service
interface. It hides a lot of the lower-level plumbing, such as
authentication, request retries, and error handling.

The SDK also includes helpful utilities. For example, the |S3|
download and upload manager can automatically break up large objects
into multiple parts and transfer them in parallel.

Use the |sdk-go-dg| to help you install, configure, and use the SDK.
The guide provides configuration information, sample code, and an
introduction to the SDK utilities.

Using the AWS SDK for Go with AWS Cloud9
========================================

AWS Cloud9 is a web-based integrated development environment (IDE) that
contains a collection of tools that you use to code, build, run, test, debug,
and release software in the cloud.

See :doc:`cloud9-go` for information on using |AC9long| with the |sdk-go|.

More Info
=========

-  To learn about everything you need before you can start using the
   |sdk-go|, see :doc:`setting-up`.
-  For code examples, see :doc:`common-examples`.
-  You can browse the |sdk-go| examples in the
   `AWS Code Sample Catalog <https://docs.aws.amazon.com/code-samples/latest/catalog/code-catalog-go.html>`_.
-  To learn about the SDK utilities, see :doc:`sdk-utilities`.
-  For learn about the types and functionality that the library provides,
   see the |sdk-go-api|_.
-  To view a video introduction of the SDK and a sample application demonstration, see
   `AWS SDK For Go: Gophers Get Going with AWS <https://www.youtube.com/watch?v=iOGIKG3EptI&feature=youtu.be>`_ from AWS
   re:Invent 2015.
