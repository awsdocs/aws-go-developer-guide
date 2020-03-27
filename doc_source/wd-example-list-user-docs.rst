.. Copyright 2010-2020 Amazon.com, Inc. or its affiliates. All Rights Reserved.

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

.. literalinclude:: workdocs.go.list_user_docs.imports.txt
   :dedent: 0
   :language: go

Get the user and organization ID from the command line.
If either is missing,
print an error message and quit.

.. literalinclude:: workdocs.go.list_user_docs.vars.txt
   :dedent: 4
   :language: go

Create a session and |WD| client.

.. literalinclude:: workdocs.go.list_user_docs.session.txt
   :dedent: 4
   :language: go

Get the root folder for the user.

.. literalinclude:: workdocs.go.list_user_docs.root_folder.txt
   :dedent: 4
   :language: go

Run the :code:`DescribeFolderContents` method and display the name, size, and
last modified information for each document.

.. literalinclude:: workdocs.go.list_user_docs.describe.txt
   :dedent: 8
   :language: go

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/workdocs/wd_list_user_docs.go>`_
on GitHub.
