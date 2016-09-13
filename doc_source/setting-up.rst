.. Copyright 2010-2016 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.


##########
Setting Up
##########

Get the latest AWS SDK for Go and the credentials required to use the
SDK.

The AWS SDK for Go requires Go 1.5 or later. You can view your current
version of Go by running the ``go version`` command. For information
about installing or upgrading your version of Go, see
https://golang.org/doc/install.

To install the SDK and its dependencies, run the following Go command:

.. code:: sh

    go get -u github.com/aws/aws-sdk-go/...

If you set the `Go vendor
experiment <https://github.com/aws/aws-sdk-go#installing>`__ environment
variable to ``1``, you can use the following command to get the SDK. The
SDK's runtime dependancies are vendored in the ``vendor/`` folder.

.. code:: sh

    go get -u github.com/aws/aws-sdk-go

    
.. _get-aws-credentials:

Get AWS Credentials
===================

The AWS SDK for Go requires AWS access keys to sign requests you send to
AWS. This is how AWS authenticates your requests. AWS access keys
consist of an access key ID and a secret access key.

Depending on the scenario, you can create IAM users to generate
long-term access keys or IAM roles to generate temporary access keys.
For more information about how and when to generate access keys, and
which type to use for your scenario, see :aws-gr:`Best Practices for Managing 
AWS Access Keys <aws-access-keys-best-practices>`.

To set up your credentials for use with the AWS SDK for Go, see :doc:`configuring-sdk`.

.. _packages:

Packages
========

After you have installed the SDK, you import AWS packages into your Go
applications to use the SDK, as shown in the following example:

.. code:: go

    import "github.com/aws/aws-sdk-go/service/s3"

For more information about the packages provided by the AWS SDK for Go,
see the list of :sdk-go-api-deep:`packages <service.html>` in
the |sdk-go-api|.

