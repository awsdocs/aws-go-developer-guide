.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _s3-examples-bucket-acls:

#############################
Working with |S3| Bucket ACLs
#############################

.. meta::
   :description: Manage Amazon S3 bucket ACLs in using this AWS SDK for Go code example.
   :keywords: AWS SDK for Go code examples, S3, ACL, permissions

The following examples use |sdk-go| functions to:

* Get the access control lists (ACLs) on a bucket
* Get the ACLs on a bucket item
* Add a new user to the ACLs on a bucket
* Add a new user to the ACLs on a bucket item

You can download complete
versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <s3>` repository on GitHub.

.. _s3-examples-bucket-acls-scenario:

Scenario
========

In these examples, a series of Go routines are used to manage ACLs
on your |S3| buckets.
The routines use the |sdk-go| to perform |S3| bucket operations using the
following methods of the |S3| client class:

* :sdk-go-api-deep:`GetBucketAcl <service/s3/#S3.GetBucketAcl>`
* :sdk-go-api-deep:`GetObjectAcl <service/s3/#S3.GetObjectAcl>`
* :sdk-go-api-deep:`PutBucketAcl <service/s3/#S3.PutBucketAcl>`
* :sdk-go-api-deep:`PutObjectAcl <service/s3/#S3.PutObjectAcl>`

.. _s3-examples-bucket-acls-prerequisites:

Prerequisites
=============

* You have :doc:`set up <setting-up>`, and :doc:`configured <configuring-sdk>` the |sdk-go|.
* You are familiar with |S3| bucket ACLs. To learn more, see :s3-dg:`Managing Access with ACLs <S3_ACLs_UsingACLs>` in the |S3-dg|.

.. _s3-examples-bucket-acls-get-bucket-acl:

Get a Bucket ACL
================

The
:sdk-go-api-deep:`GetBucketAcl <service/s3/#S3.GetBucketAcl>`
function gets the ACLs on a bucket.

The following example gets the ACLs on a bucket with the name specified as
a command line argument.

Create the file *s3_get_bucket_acl.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_get_bucket_acl.go
   :lines: 17-23
   :language: go

Create a function to display errors and exit.

.. literalinclude:: example_code/s3/s3_get_bucket_acl.go
   :lines: 69-72
   :language: go

This example requires one input parameter, the name of the bucket.
If the name is not supplied, call the error function and exit.

.. literalinclude:: example_code/s3/s3_get_bucket_acl.go
   :lines: 30-34
   :language: go
   :dedent: 4

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials*,
the region from the shared configuration file *~/.aws/config*,
and create a new |S3| service client.

.. literalinclude:: example_code/s3/s3_get_bucket_acl.go
   :lines: 38-43
   :language: go
   :dedent: 4

Call :sdk-go-api-deep:`GetBucketAcl <service/s3/#S3.GetBucketAcl>`,
passing in the name of the bucket.
If an error occurs, call ``exitErrorf``.
If no error occurs, loop through the results
and print out the name, type, and permission for the grantees.

.. literalinclude:: example_code/s3/s3_get_bucket_acl.go
   :lines: 46-66
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_get_bucket_acl.go>`_
on GitHub.

.. _s3-examples-bucket-acls-set-bucket-acl:

Set a Bucket ACL
================

The
:sdk-go-api-deep:`PutBucketAcl <service/s3/#S3.PutBucketAcl>`
function sets the ACLs on a bucket.

The following example gives a user access by email address to a bucket
with the bucket name and email address specified as command line arguments.
The user can also supply a permission argument. However,
if it isn'o't supplied, the user is given READ access to the bucket.

Create the file *s3_put_bucket_acl.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_put_bucket_acl.go
   :lines: 17-23
   :language: go

Create a function to display errors and exit.

.. literalinclude:: example_code/s3/s3_put_bucket_acl.go
   :lines: 101-104
   :language: go

Get the two required input parameters.
If the optional permission parameter is supplied,
make sure it is one of the allowed values.
If not, print an error message and quit.

.. literalinclude:: example_code/s3/s3_put_bucket_acl.go
   :lines: 31-49
   :language: go
   :dedent: 4

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials*,
the region from the shared configuration file *~/.aws/config*,
and create a new |S3| service client.

.. literalinclude:: example_code/s3/s3_put_bucket_acl.go
   :lines: 55-60
   :language: go
   :dedent: 4

Get the existing ACLs and append the new user to the list.
If there is an error while retrieving the list,
print an error message and quit.

.. literalinclude:: example_code/s3/s3_put_bucket_acl.go
   :lines: 63-79
   :language: go
   :dedent: 4

Build the parameter list for the call based on the existing
ACLs and the new user information.

.. literalinclude:: example_code/s3/s3_put_bucket_acl.go
   :lines: 81-90
   :language: go
   :dedent: 4

Call :sdk-go-api-deep:`PutBucketAcl <service/s3/#S3.PutBucketAcl>`,
passing in the parameter list.
If an error occurs, display a message and quit.
Otherwise, display a message indicating success.

.. literalinclude:: example_code/s3/s3_put_bucket_acl.go
   :lines: 93-98
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_put_bucket_acl.go>`_
on GitHub.

.. _s3-examples-bucket-acls-make-bucket-public-acl:

Making a Bucket Public using a Canned ACL
=========================================

As in the previous example, the
:sdk-go-api-deep:`PutBucketAcl <service/s3/#S3.PutBucketAcl>`
function sets the ACLs on a bucket.

The following example gives everyone read-only access to the items in the bucket
with the bucket name specified as a command line argument.

Create the file *s3_make_bucket_public.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_make_bucket_public.go
   :lines: 17-23
   :dedent: 0
   :language: go

Create a function to display errors and exit.

.. literalinclude:: example_code/s3/s3_make_bucket_public.go
   :lines: 25-28
   :dedent: 0
   :language: go

Get the required input parameter and create the **acl**.

.. literalinclude:: example_code/s3/s3_make_bucket_public.go
   :lines: 35-39, 43
   :dedent: 4
   :language: go

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials*
the region from the shared configuration file *~/.aws/config*,
and create a new |S3| service client.

.. literalinclude:: example_code/s3/s3_make_bucket_public.go
   :lines: 48-50, 53
   :language: go
   :dedent: 4

Create the input for and call :sdk-go-api-deep:`PutBucketAcl <service/s3/#S3.PutBucketAcl>`.
If an error occurs, display a message and quit.
Otherwise, display a message indicating success.

.. literalinclude:: example_code/s3/s3_make_bucket_public.go
   :lines: 55-66
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_make_bucket_public.go>`_
on GitHub.

.. _s3-examples-bucket-acls-get-bucket-object-acl:

Get a Bucket Object ACL
=======================

The
:sdk-go-api-deep:`PutObjectAcl <service/s3/#S3.PutObjectAcl>`
function sets the ACLs on a bucket item.

The following example gets the ACLs for a bucket item
with the bucket and item name specified as command line arguments.

Create the file *s3_get_bucket_object_acl.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_get_bucket_object_acl.go
   :lines: 17-23
   :language: go

Create a function to display errors and exit.

.. literalinclude:: example_code/s3/s3_get_bucket_object_acl.go
   :lines: 64-67
   :language: go

This example requires two input parameters, the names of the bucket and object.
If either name is not supplied, call the error function and exit.

.. literalinclude:: example_code/s3/s3_get_bucket_object_acl.go
   :lines: 30-35
   :language: go
   :dedent: 4

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials*,
the region from the shared configuration file *~/.aws/config*,
and create a new |S3| service client.

.. literalinclude:: example_code/s3/s3_get_bucket_object_acl.go
   :lines: 39-44
   :language: go
   :dedent: 4

Call :sdk-go-api-deep:`GetObjectAcl <service/s3/#S3.GetObjectAcl>`,
passing in the names of the bucket and object.
If an error occurs, call ``exitErrorf``.
If no error occurs, loop through the results
and print out the name, type, and permission for the grantees.

.. literalinclude:: example_code/s3/s3_get_bucket_object_acl.go
   :lines: 47-61
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_get_bucket_object_acl.go>`_
on GitHub.

.. _s3-examples-bucket-acls-set-bucket-object-acl:

Set a Bucket Object ACL
=======================

The
:sdk-go-api-deep:`PutObjectAcl <service/s3/#S3.PutObjectAcl>`
function sets the ACLs on a bucket item.

The following example gives a user access by email address to a bucket item,
with the bucket and item names and email address specified as command line arguments.
The user can also supply a permission argument.
However, if it isn't supplied, the user is given READ access to the bucket.

Create the file *s3_put_bucket_object_acl.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_put_bucket_object_acl.go
   :lines: 17-23
   :language: go

Create a function to display errors and exit.

.. literalinclude:: example_code/s3/s3_put_bucket_object_acl.go
   :lines: 101-104
   :language: go

Get the three required input parameters.
If the optional permission parameter is supplied,
make sure it is one of the allowed values.
If not, print an error message and quit.

.. literalinclude:: example_code/s3/s3_put_bucket_object_acl.go
   :lines: 31-49
   :language: go
   :dedent: 4

Initialize the session that the SDK uses to load credentials
from the shared credentials file *~/.aws/credentials,
and create a new |S3| service client.

.. literalinclude:: example_code/s3/s3_put_bucket_object_acl.go
   :lines: 53-58
   :language: go
   :dedent: 4

Build the parameter list for the call based on the existing
ACLs and the new user information.

.. literalinclude:: example_code/s3/s3_put_bucket_object_acl.go
   :lines: 61-90
   :language: go
   :dedent: 4

Call :sdk-go-api-deep:`PutObjectAcl <service/s3/#S3.PutObjectAcl>`,
passing in the parameter list.
If an error occurs, display a message and quit.
Otherwise, display a message indicating success.

.. literalinclude:: example_code/s3/s3_put_bucket_object_acl.go
   :lines: 93-98
   :language: go
   :dedent: 4

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_put_bucket_object_acl.go>`_
on GitHub.
