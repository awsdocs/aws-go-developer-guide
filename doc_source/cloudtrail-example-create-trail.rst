.. Copyright 2010-2020 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _cloudtrail-example-create-trail:

#####################
Creating a |CT| Trail
#####################

.. meta::
    :description:
        Create an |CTlong| trail using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, CloudTrail

This example uses the
:sdk-go-api-deep:`CreateTrail <service/cloudtrail/#CloudTrail.CreateTrail>` operation
to create a |CT| trail
in the :code:`us-west-2` region.
It requires two inputs, the name of the trail and the name of the bucket
in which |CT| stores information.
If the bucket does not have the proper policy,
include the **-p** flag to attach the correct policy to the bucket.

Choose :code:`Copy` to save the code locally.

Create the file *create_trail.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: cloudtrail.go.create_trail.imports.txt
   :dedent: 0
   :language: go

Get the names of the trail and bucket,
and whether to attach the policy to the bucket.
If either the trail name or bucket name is missing,
display an error message and exit.

.. literalinclude:: cloudtrail.go.create_trail.vars.txt
   :dedent: 4
   :language: go

Initialize the session that the SDK uses to load credentials
from the shared credentials file
*.aws/credentials* in your home folder.

.. literalinclude:: cloudtrail.go.create_trail.session.txt
   :dedent: 4
   :language: go

If the **-p** flag was specfied, add a policy to the bucket.

.. literalinclude:: cloudtrail.go.create_trail.policy.txt
   :dedent: 4
   :language: go

Create the |CT| client, the input for **CreateTrail**,
and call **CreateTrail**.
If an error occurs, print the error and exit.
If no error occurs, print a success message.

.. literalinclude:: cloudtrail.go.create_trail.create.txt
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudtrail/create_trail.go>`_
on GitHub.
