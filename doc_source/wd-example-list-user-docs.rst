.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _wd-example-list-user-docs:

#################
Listing User Docs
#################

The following example lists the documents for the user whose name is specified
on the command line.
Choose :code:`Copy` to save the code locally, or see the link to the complete
example at the end of this topic.

Import the following Go packages.

.. literalinclude:: ./example_code/workdocs/wd_list_user_docs.go
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

Check that we have a user name, and get the root folder for that user.

.. literalinclude:: ./example_code/workdocs/wd_list_user_docs.go
   :lines: 42-70
   :dedent: 4
   :language: go

Run the :code:`DescribeFolderContents` method and display the name, size, and
last modified information for each document.

.. literalinclude:: ./example_code/workdocs/wd_list_user_docs.go
   :lines: 72-87
   :dedent: 8
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/workdocs/wd_list_user_docs.go>`_
on GitHub.
