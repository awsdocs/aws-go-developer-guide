.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

#####################
What is the |sdk-go|?
#####################

.. meta::
   :description: Use the |sdk-go| to build Go applications that use AWS services.

The |sdk-go| provides APIs and utilities that developers can use
to build Go applications that use AWS services, such as |EC2long| (|EC2|) and 
|S3long| (|S3|).

The SDK removes the complexity of coding directly against a web service
interface. It hides a lot of the lower-level plumbing, such as
authentication, request retries, and error handling.

The SDK also includes helpful utilities. For example, the |S3|
download and upload manager can automatically break up large objects
into multiple parts and transfer them in parallel.

.. _about-the-go-sdk-dev-guide:

About the |sdk-go| Developer Guide
==================================

Use the developer guide to help you install, configure, and use the SDK.
The guide provides configuration information, sample code, and an
introduction to the SDK utilities.

-  For information about everything you need before you start using the
   |sdk-go|, see :doc:`setting-up`.
-  For code examples, see :doc:`common-examples`.
-  For information about the SDK utilities, see :doc:`sdk-utilities`.
-  For information about the types and functionality provided by the
   library, see the |sdk-go-api|_.
-  To view a video introduction of the SDK and a sample application demonstration, see 
   `AWS SDK For Go: Gophers Get Going with AWS <https://www.youtube.com/watch?v=iOGIKG3EptI&feature=youtu.be>`_ from AWS 
   re:Invent 2015.
