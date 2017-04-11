.. Copyright 2010-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _s3-examples-bucket-acls:

###################################
Working with |S3| Bucket ACLs in Go
###################################

.. meta::
   :description: Use S3 code examples to manage |S3| bucket ACLs in
                 Go applications.
   :keywords: AWS SDK for Go examples, |S3|, ACL, permissions

The following examples use |sdk-go| functions to:

* Get the ACLs on a bucket
* Get the ACLs on a bucket item
* Add a new user to the ACLs on a bucket
* Add a new user to the ACLs on a bucket item

You can download complete
versions of these example files from the
:doc-examples-go:`aws-doc-sdk-examples <s3>` repository on GitHub.

.. _s3-examples-bucket-acls-scenario:

The Scenario
============

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

Getting a Bucket ACL
====================

This example gets the ACLs for a bucket.
One command-line argument is required,
the name of the bucket.

Create the file *s3_get_bucket_acl.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_get_bucket_acl.go
   :lines: 15-23

Create a function to display errors and exit.

.. literalinclude:: example_code/s3/s3_get_bucket_acl.go
   :lines: 64-67

This example requires one input parameter, the name of the bucket.
If the name is not supplied, we call the error function and exit.

.. literalinclude:: example_code/s3/s3_get_bucket_acl.go
   :lines: 29-34

Initialize the session that the SDK uses to load configuration, credential,
and region information from the shared config file *~/.aws/config*,
and create a new |S3| service client.

.. literalinclude:: example_code/s3/s3_get_bucket_acl.go
   :lines: 38-43

Call :sdk-go-api-deep:`GetBucketAcl <service/s3/#S3.GetBucketAcl>`,
passing in the name of the bucket.
If an error occurs, call ``exitErrorf``.
If no error occurs, loop through the results
and print out the name, type, and permssion for the grantees.

.. literalinclude:: example_code/s3/s3_get_bucket_acl.go
   :lines: 46-61

See the `complete example 
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_get_bucket_acl.go>`_
on GitHub.

.. _s3-examples-bucket-acls-set-bucket-acl:

Setting a Bucket ACL
====================

This example gives a user, by email address, access to a bucket.
There are two required input parameters, the bucket name 
and the email address of the user.
In addition, the user can supply an optional permission argumement.

Create the file *s3_put_bucket_acl.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_put_bucket_acl.go
   :lines: 15-23

Create a function to display errors and exit.

.. literalinclude:: example_code/s3/s3_put_bucket_acl.go
   :lines: 102-105

Get the two required input parameters.
If the optional permission parameter is supplied,
make sure it is one of the allowed values.
If not, print an error message and quit.

.. literalinclude:: example_code/s3/s3_put_bucket_acl.go
   :lines: 30-48

Initialize the session that the SDK uses to load configuration, credential,
and region information from the shared config file *~/.aws/config*,
and create a new |S3| service client.

.. literalinclude:: example_code/s3/s3_put_bucket_acl.go
   :lines: 54-59

Get the existing ACLs and append the new user to the list.
If we encounter an error while retrieving the list,
print an error message and quit.

.. literalinclude:: example_code/s3/s3_put_bucket_acl.go
   :lines: 62-79

Build the parameter list for the call based on the existing
ACLs and the new user information.

.. literalinclude:: example_code/s3/s3_put_bucket_acl.go
   :lines: 81-90

Call :sdk-go-api-deep:`PutBucketAcl <service/s3/#S3.PutBucketAcl>`,
passing in the parameter list.
If an error occurs, display a message and quit.
Otherwise display a message indicating success.

.. literalinclude:: example_code/s3/s3_put_bucket_acl.go
   :lines: 93-99

See the `complete example 
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_put_bucket_acl.go>`_
on GitHub.

.. _s3-examples-bucket-acls-get-bucket-object-acl:

Getting a Bucket Object ACL
===========================

This example gets the ACLs for a bucket object.
Two command-line arguments are required,
the names of the bucket and object.

Create the file *s3_get_bucket_object_acl.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_get_bucket_objec_acl.go
   :lines: 15-23

Create a function to display errors and exit.

.. literalinclude:: example_code/s3/s3_get_bucket_objec_acl.go
   :lines: 65-68

This example requires two input parameters, the names of the bucket and object.
If either name is not supplied, call the error function and exit.

.. literalinclude:: example_code/s3/s3_get_bucket_objec_acl.go
   :lines: 29-35

Initialize the session that the SDK uses to load configuration, credential,
and region information from the shared config file *~/.aws/config*,
and create a new |S3| service client.

.. literalinclude:: example_code/s3/s3_get_bucket_objec_acl.go
   :lines: 39-44

Call :sdk-go-api-deep:`GetObjectAcl <service/s3/#S3.GetObjectAcl>`,
passing in the names of the bucket and object.
If an error occurs, call ``exitErrorf``.
If no error occurs, loop through the results
and print out the name, type, and permssion for the grantees.

.. literalinclude:: example_code/s3/s3_get_bucket_objec_acl.go
   :lines: 47-62

See the `complete example 
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_get_bucket_object_acl.go>`_
on GitHub.

.. _s3-examples-bucket-acls-set-bucket-object-acl:

Setting a Bucket Object ACL
===========================

This example gives a user, by email address, access to a bucket object.
There are three required input parameters, the bucket name, the object name,
and the email address of the user.
In addition, the user can supply an optional permission argumement.

Create the file *s3_put_bucket_object_acl.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: example_code/s3/s3_put_bucket_object_acl.go
   :lines: 15-23

Create a function to display errors and exit.

.. literalinclude:: example_code/s3/s3_put_bucket_object_acl.go
   :lines: 104-107

Get the three required input parameters.
If the optional permission parameter is supplied,
make sure it is one of the allowed values.
If not, print an error message and quit.

.. literalinclude:: example_code/s3/s3_put_bucket_object_acl.go
   :lines: 30-49

Initialize the session that the SDK uses to load configuration, credential,
and region information from the shared config file *~/.aws/config*,
and create a new |S3| service client.

.. literalinclude:: example_code/s3/s3_put_bucket_object_acl.go
   :lines: 55-60

Build the parameter list for the call based on the existing
ACLs and the new user information.

.. literalinclude:: example_code/s3/s3_put_bucket_object_acl.go
   :lines: 51, 63-92

Call :sdk-go-api-deep:`PutObjectAcl <service/s3/#S3.PutObjectAcl>`,
passing in the parameter list.
If an error occurs, display a message and quit.
Otherwise display a message indicating success.

.. literalinclude:: example_code/s3/s3_put_bucket_object_acl.go
   :lines: 95-101

See the `complete example 
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/s3/s3_put_bucket_object_acl.go>`_
on GitHub.
