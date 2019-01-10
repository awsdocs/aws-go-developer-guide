.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _define_metrics:

#####################
Definitions for |CSM|
#####################

.. meta::
   :description: Configure an agent for |CSM| for Enterprise Support with the |sdk|.
   :keywords: |sdk|, |CSM| for Enterprise Support with |language|, use |language| to monitor AWS Services

Use the following descriptions of |CSM| to interpret your results.
In general, these metrics are available for review with your
Technical Account Manager during regular business reviews.
AWS Support resources and your Technical Account Manager
should have access to |CSM| data to help you resolve cases,
but if you discover data that is confusing or unexpected,
but doesn't seem to be negatively impacting your application's performance,
it is best to review that data during scheduled business reviews.

.. list-table:: Metrics
   :header-rows: 1

   * - Metric
     - Definition
     - How to use it
   * - CallCount
     - Total number of successful or failed API calls from your code to AWS services
     - Use it as a baseline to correlate with other metrics like errors or throttling.
   * - ClientErrorCount
     - Number of API calls that fail with client errors (4xx HTTP response codes).
       Examples: Throttling, Access denied, S3 bucket does not exist, and Invalid parameter value.
     - Except in certain cases related to throttling
       (ex. when throttling occurs due to a limit that needs to be increased)
       this metric can indicate something in your application that needs to be fixed.
   * - ConnectionErrorCount
     - Number of API calls that fail because of errors connecting to the service.
       These can be caused by network issues between the customer application
       and AWS services including load balancers, DNS failures, transit providers.
       In some cases, AWS issues may result in this error.
     - Use this metric to determine whether issues are specific to your application
       or are caused by your infrastructure and/or network.
       High ConnectionErrorCount could also indicate short timeout values for API calls.
   * - ThrottleCount
     - Number of API calls that fail due to throttling by AWS services.
     - Use this metric to assess if your application has reached throttle limits,
       as well as to determine the cause of retries and application latency.
       Consider distributing calls over a window instead of batching your calls.
   * - ServerErrorCount
     - Number of API calls that fail due to server errors (5xx HTTP response codes) from AWS Services.
       These are typically caused by AWS services.
     - Determine cause of SDK retries or latency.
       This metric will not always indicate that AWS services are at fault,
       as some AWS teams classify latency as an HTTP 503 response.
   * - EndToEndLatency
     - Total time for your application to make a call using the AWS SDK,
       inclusive of retries.
       In other words, regardless of whether it is successful after several attempts,
       or as soon as a call fails due to an unretriable error.
     - Determine how AWS API calls contribute to your application's overall latency.
       Higher than expected latency may be caused by issues with network, firewall,
       or other configuration settings, or by latency that occurs as a result of SDK retries. 
