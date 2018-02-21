.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _aws-go-sdk-ses-example-get-statistics:

########################
Getting |SES| Statistics
########################

.. meta::
    :description:
        Learn how to get statistics about Amazon SES through this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, Amazon SES

The following example demonstrates how to use the |sdk-go| to get statistics
about |SES|. Use this information to avoid damaging your reputation when emails
are bounced or rejected.

.. literalinclude:: ./example_code/ses/ses_get_statistics.go
   :lines: 15-75
   :dedent: 0
   :language: go

See the `complete example <https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/ses/ses_get_statistics.go>`_
on GitHub.
