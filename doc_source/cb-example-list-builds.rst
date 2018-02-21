.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _cb-example-list-builds:

#########################################
Listing Your AWS CodeBuild Project Builds
#########################################

.. meta::
    :description:
        List AWS CodeBuild projects using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, CodeBuild

The following example displays information about your |ACBlong| project builds,
including the name of the project, when the build started, and how long each
phase of the build took, in seconds.

.. literalinclude:: ./example_code/codebuild/cb_list_builds.go
   :lines: 15-58
   :dedent: 0
   :language: go

Choose :code:`Copy` to save the code locally.
See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/codebuild/cb_list_builds.go>`_
on GitHub.
