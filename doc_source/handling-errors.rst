.. Copyright 2010-2016 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.


###############
Handling Errors
###############

The AWS SDK for Go returns errors that satisfy the Go ``error``
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
interface to retrieve the upload ID, which is helpful when you must
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



.. toctree::
   :titlesonly:
   :maxdepth: 1

   handle-service-error-codes


.. meta::
   :description: Use the Error interface to handle errors from the |sdk-go| or AWS service.
   :keywords: errors, error handling, error interface
