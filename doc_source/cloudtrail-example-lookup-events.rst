.. Copyright 2010-2020 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _cloudtrail-example-lookup-events:

#########################
Listing |CT| Trail Events
#########################

.. meta::
    :description:
        List the |CTlong| trail events using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, CloudTrail

This example uses the
:sdk-go-api-deep:`LookupEvents <service/cloudtrail/#CloudTrail.LookupEvents>` operation
to list the |CT| trail events
in the :code:`us-west-2` region.

Choose :code:`Copy` to save the code locally.

Create the file *lookup_events.go*.
Add the following statements to import the Go and |sdk-go| packages used in the example.

.. literalinclude:: cloudtrail.go.lookup_events.imports.txt
   :dedent: 0
   :language: go

Get the name of the trail.
If the trail name is missing,
display an error message and exit.

.. literalinclude:: cloudtrail.go.lookup_events.vars.txt
   :dedent: 4
   :language: go

Initialize the session that the SDK uses to load credentials
from the shared credentials file
*.aws/credentials* in your home folder, and create a new service client.

.. literalinclude:: cloudtrail.go.lookup_events.session.txt
   :dedent: 4
   :language: go

Create the |CT| client, and the input for and call **LookupEvents**.
If an error occurs, print the error and exit.
If no error occurs, loop through the events,
printing information about each event.

.. literalinclude:: cloudtrail.go.lookup_events.lookup.txt
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/cloudtrail/lookup_events.go>`_
on GitHub.
