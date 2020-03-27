.. Copyright 2010-2020 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _wd-example-list-users:

#############
Listing Users
#############

The following example lists the names of all users, or lists additional details
about a user if a user name is specified on the command line.
Choose :code:`Copy` to save the code locally, or see the link to the complete
example at the end of this topic.

Import the following Go packages.

.. literalinclude:: workdocs.go.list_users.imports.txt
   :dedent: 0
   :language: go

Get an optional user name and organization ID from the command line.
If the organization ID is missing,
print an error message and quit.

.. literalinclude:: workdocs.go.list_users.vars.txt
   :dedent: 4
   :language: go

Create input for the :code:`DescribeUsers` method.

.. literalinclude:: workdocs.go.list_users.input.txt
   :dedent: 4
   :language: go

Create a session and |WD| client.

.. literalinclude:: workdocs.go.list_users.session.txt
   :dedent: 4
   :language: go

Call :code:`DescribeUsers` and display the information for the user or
all users.

.. literalinclude:: workdocs.go.list_users.describe.txt
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/workdocs/wd_list_users.go>`_
on GitHub.
