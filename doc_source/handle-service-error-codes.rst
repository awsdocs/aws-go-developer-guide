.. Copyright 2010-2016 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _handling-specific-service-error-codes:

#####################################
Handling Specific Service Error Codes
#####################################

.. meta::
   :description: Handle error codes returned by AWS services in the |sdk-go|.
   :keywords: errors, error handling, error interface

The example below demonstrates how to handle error codes that you encounter while using the 
|sdk-go|. The example assumes you have already set up and configured the SDK (that
is, all required packages are imported and your credentials and region
are set). For more information, see :doc:`setting-up` and :doc:`configuring-sdk`.

This example highlights how you can use the `awserr.Error` type to perform logic based on specific error codes 
returned by service API operations.

In this example the `S3` `GetObject` API operation is used to request the contents of an object in S3. The 
example handles the `NoSuchBucket` and `NoSuchKey` error codes, printing custom messages to stderr. If any 
other error is received, a generic message is printed.

.. literalinclude:: aws/request/handleServiceErrorCodes/handleServiceErrorCodes.go
   :lines: 39-62
   :dedent: 1

You can see the complete example code on :go-sdk-examples:`GitHub <aws/request/handleServiceErrorCodes/handleServiceErrorCodes.go>`.

