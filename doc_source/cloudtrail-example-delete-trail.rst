.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _cloudtrail-example-delete-trail:

#####################
Deleting a |CT| Trail
#####################

.. meta::
    :description:
        Delete an |CTlong| trail using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, CloudTrail

This example uses the
:sdk-go-api-deep:`DeleteTrail <service/cloudtrail/#CloudTrail.DeleteTrail>` operation
to delete a |CT| trail
in the :code:`us-west-2` region.
It requires one input, the name of the trail.

Choose :code:`Copy` to save the code locally.

Create the file *delete_trail.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: ./example_code/cloudtrail/delete_trail.go
   :lines: 17-25
   :dedent: 0
   :language: go

Get the name of the trail.
If the trail name is missing,
display an error message and exit.

.. literalinclude:: ./example_code/cloudtrail/delete_trail.go
   :lines: 29-38
   :dedent: 4
   :language: go

Initialize the session that the SDK uses to load credentials
from the shared credentials file
*.aws/credentials* in your home folder
and create the client.

.. literalinclude:: ./example_code/cloudtrail/delete_trail.go
   :lines: 41-46
   :dedent: 4
   :language: go

Call **DeleteTrail** with the trail name.
If an error occurs, print the error and exit.
If no error occurs, print a success message.

.. literalinclude:: ./example_code/cloudtrail/delete_trail.go
   :lines: 48-55
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudtrail/delete_trail.go>`_
on GitHub.
