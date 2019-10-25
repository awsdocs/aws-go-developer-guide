.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.


#################################
Getting Started with the |sdk-go|
#################################

.. meta::
   :description: Get the latest |sdk-go| and the credentials required to use the SDK.
   :keywords:

The |sdk-go| requires Go 1.5 or later. You can view your current
version of Go by running the ``go version`` command. For information
about installing or upgrading your version of Go, see
https://golang.org/doc/install.

.. _get_amazon_account:

Get an Amazon Account
=====================

Before you can use the |sdk-go|,
you must have an Amazon account.
See
`How do I create and activate a new Amazon Web Services account? <https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account>`_
for details.

.. _install_go_sdk:

Install the |sdk-go|
====================

To install the SDK and its dependencies, run the following Go command.

.. code:: sh

    go get -u github.com/aws/aws-sdk-go/...

If you set the `Go vendor experiment <https://github.com/aws/aws-sdk-go/blob/master/README.md#installing>`_
environment variable to ``1``, you can use the following command to get the SDK. The
SDK's runtime dependencies are vendored in the ``vendor/`` folder.

.. code:: sh

    go get -u github.com/aws/aws-sdk-go


.. _get-aws-credentials:

Get your AWS access keys
========================

.. include:: common/procedure-get-access-keys.txt

.. _packages:

Import Packages
===============

After you have installed the SDK, you import AWS packages into your Go
applications to use the SDK, as shown in the following example,
which imports the AWS, Session, and |S3| libraries:

.. code:: go

   import (
       "github.com/aws/aws-sdk-go/aws"
       "github.com/aws/aws-sdk-go/aws/session"
       "github.com/aws/aws-sdk-go/service/s3"
   )
