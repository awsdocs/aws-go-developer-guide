.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _cloudtrail-example-describe-trails:

#######################
Listing the |CT| Trails
#######################

.. meta::
    :description:
        List the |CTlong| trails using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, CloudTrail

This example uses the
:sdk-go-api-deep:`DescribeTrails <service/cloudtrail/#CloudTrail.DescribeTrails>` operation
to list the names of the |CT| trails
and the bucket in which |CT| stores information
in the :code:`us-west-2` region.

Choose :code:`Copy` to save the code locally.

Create the file *describe_trails.go*.
Add the following statements to import the Go and AWS SDK for Go packages used in the example.

.. literalinclude:: ./example_code/cloudtrail/describe_trails.go
   :lines: 17-24
   :dedent: 0
   :language: go

Initialize the session that the SDK uses to load credentials
from the shared credentials file
*.aws/credentials* in your home folder, and create a new service client.

.. literalinclude:: ./example_code/cloudtrail/describe_trails.go
   :lines: 29-34
   :dedent: 4
   :language: go

Call **DescribeTrails**.
If an error occurs, print the error and exit.
If no error occurs, loop through the trails,
printing the name of each trail and the bucket.

.. literalinclude:: ./example_code/cloudtrail/describe_trails.go
   :lines: 36-51
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudtrail/describe_trails.go>`_
on GitHub.
