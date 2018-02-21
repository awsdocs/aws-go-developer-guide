.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

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

.. literalinclude:: ./example_code/workdocs/wd_list_users.go
   :lines: 17-23
   :dedent: 0
   :language: go

- :code:`flag` is for getting user input, in this case the name of the user
- :code:`fmt` is for formatting output
- :code:`session` is for creating a session
- :code:`workdocs` is for using the WorkDocs APIs

Create a session and |WD| client.

.. literalinclude:: ./example_code/workdocs/wd_list_users.go
   :lines: 35-40
   :dedent: 4
   :language: go

Create the input arguments for the :code:`DescribeUsers` method and add your
organization ID.

.. literalinclude:: ./example_code/workdocs/wd_list_users.go
   :lines: 42-46
   :dedent: 4
   :language: go

If we have a user name, add that to the input arguments so we only get information
about that user.

.. literalinclude:: ./example_code/workdocs/wd_list_users.go
   :lines: 48-57
   :dedent: 4
   :language: go

Run the :code:`DescribeUsers` method and display the information for the user or
all users.

.. literalinclude:: ./example_code/workdocs/wd_list_users.go
   :lines: 61-84
   :dedent: 4
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/workdocs/wd_list_users.go>`_
on GitHub.
