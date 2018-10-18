.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.


###############################
Handling Errors in the |sdk-go|
###############################

.. meta::
   :description: Use the Error interface to handle errors from the |sdk-go| or AWS service.
   :keywords: errors, error handling, error interface

The |sdk-go| returns errors that satisfy the Go ``error``
interface type and the :sdk-go-api-deep:`Error <aws/awserr/#Error>` interface in the
``aws/awserr`` package. You can use the ``Error()`` method to get a formatted string of
the SDK error message without any special handling.

.. code:: go

    if err != nil {
        if awsErr, ok := err.(awserr.Error); ok {
            // process SDK error
        }
    }

Errors returned by the SDK are backed by a concrete type that will
satisfy the ``awserr.Error`` interface. The interface has the following
methods, which provide classification and information about the error.

-  ``Code`` returns the classification code by which related errors are
   grouped.
-  ``Message`` returns a description of the error.
-  ``OrigErr`` returns the original error of type ``error`` that is
   wrapped by the ``awserr.Error`` interface, such as a standard library
   error or a service error.

Handling Specific Service Error Codes
=====================================

.. meta::
   :description: Handle error codes returned by AWS services in the |sdk-go|.
   :keywords: errors, error handling, error interface

The following example demonstrates how to handle error codes that you encounter while using the
|sdk-go|. The example assumes you have already set up and configured the SDK (that
is, all required packages are imported and your credentials and region
are set). For more information, see :doc:`setting-up` and :doc:`configuring-sdk`.

This example highlights how you can use the `awserr.Error` type to perform logic based on specific error codes
returned by service API operations.

In this example the `S3` `GetObject` API operation is used to request the contents of an object in S3. The
example handles the `NoSuchBucket` and `NoSuchKey` error codes, printing custom messages to stderr. If any
other error is received, a generic message is printed.

.. literalinclude:: example_code/extending_sdk/handleServiceErrorCodes.go
   :lines: 49-71
   :dedent: 4

See the `complete example <https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/extending_sdk/handleServiceErrorCodes.go>`_
on GitHub.

.. _additional-error-information:

Additional Error Information
============================

In addition to the ``awserr.Error`` interface, you might be able to use
other interfaces to get more information about an error.

Specific Error Interfaces
-------------------------

Other packages might provide their own error interfaces. For example,
the :sdk-go-api-deep:`service/s3/s3manager <service/s3/s3manager>` package
provides a :sdk-go-api-deep:`MultiUploadFailure <service/s3/s3manager/#MultiUploadFailure>`
interface to retrieve the upload ID. This is helpful when you must
manually clean up a failed multi-part upload.

.. code:: go

    output, err := s3manager.Upload(svc, input, opts)
    if err != nil {
        if multierr, ok := err.(MultiUploadFailure); ok {
            // Process error and its associated uploadID
            fmt.Println("Error:", multierr.Code(), multierr.Message(), multierr.UploadID())
        } else {
            // Process error generically
            fmt.Println("Error:", err.Error())
        }
    }

For more information, see the
:sdk-go-api-deep:`s3Manager.MultiUploadFailure <service/s3/s3manager/#MultiUploadFailure>`
interface in the |sdk-go-api|.
