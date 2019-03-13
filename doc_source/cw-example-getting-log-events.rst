.. Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _examples-cw-getting-log-eventss:

############################
Getting Log Events from |CW|
############################

.. meta::
   :description: Retrieve the events for a |CW| log group's log stream
                 using this |sdk-go| code example.
   :keywords: |sdk-go| code examples, CloudWatch, 

The following example lists up to 100 of the latest events for a log group's log stream.
Replace LOG-GROUP-NAME with the name of the |CW| log group and
LOG-STREAM-NAME with the name of the log stream for the log group.

.. literalinclude:: cloudwatch.go.getlogevents.txt
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudwatch/CloudWatchGetLogEvents.go>`_
on GitHub.
