.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _cloud9-go:

#################################
Using |AC9long| with the |sdk-go|
#################################

.. meta::
    :description:
        Describes how to use AWS Cloud9 with the AWS SDK for Go.

You can use |AC9long| with the |sdk-go| to write and run your Go code using just a browser. |AC9| includes tools such as a
code editor and terminal. Because the |AC9| IDE is cloud based, you can work on your projects from your office, home,
or anywhere using an internet-connected machine. For general information about |AC9|, see the |AC9-ug|.

Follow these instructions to set up |AC9| with the |sdk-go|:

* :ref:`cloud9-go-account`
* :ref:`cloud9-go-environment`
* :ref:`cloud9-go-sdk`
* :ref:`cloud9-go-examples`
* :ref:`cloud9-go-run`

.. _cloud9-go-account:

Step 1: Set up Your AWS Account to Use |AC9|
============================================

Start to use |AC9| by signing in to the |AC9| console as an |IAMlong| (|IAM|) entity (for example, an |IAM| user) in your AWS account who
has access permissions for |AC9|.

To set up an |IAM| entity in your AWS account to access |AC9|, and to sign in to the |AC9| console, see
`Team Setup for AWS Cloud9 <https://docs.aws.amazon.com/cloud9/latest/user-guide/setup.html>`_ in the |AC9-ug|.

.. _cloud9-go-environment:

Step 2: Set up Your |AC9| Development Environment
=================================================

After you sign in to the |AC9| console, use the console to create an |AC9| development environment.
After you create the environment, |AC9| opens the IDE for that environment.

See `Creating an Environment in AWS Cloud9 <https://docs.aws.amazon.com/cloud9/latest/user-guide/create-environment.html>`_ in the |AC9-ug| for details.

.. note:: As you create your environment in the console for the first time, we recommend that you choose the option to :guilabel:`Create a new instance for environment (EC2)`.
   This option tells |AC9| to create an environment, launch an |EC2| instance, and then connect the new instance to the new environment. This is the fastest way
   to begin using |AC9|.

.. _cloud9-go-sdk:

Step 3: Set up the |sdk-go|
===========================

After |AC9| opens the IDE for your development environment, use the IDE to set up the |sdk-go| in your environment, as follows.

#. If the terminal isn't already open in the IDE, open it. On the menu bar in the IDE, choose :guilabel:`Window, New Terminal`.
#. Set your GOPATH environment variable. To do this, add the following code
   to the end of your shell profile file (for example, :file:`~/.bashrc` in Amazon Linux, assuming you
   chose the option to :guilabel:`Create a new instance for environment (EC2)`, earlier in this topic), and then save the file.

   .. code-block:: sh

      GOPATH=~/environment/go

      export GOPATH

   After you save the file, source the :file:`~/.bashrc` file to finish setting your GOPATH environment variable. To do this,
   run the following command. (This command assumes you chose the option to :guilabel:`Create a new instance for environment (EC2)`,
   earlier in this topic.)

   .. code-block:: sh

      . ~/.bashrc

#. Run the following command to install the |sdk-go|.

   .. code-block:: sh

      go get -u github.com/aws/aws-sdk-go/...

If the IDE can't find Go, run the following commands, one at a time in this order, to install it. (These commands assume you
chose the option to :guilabel:`Create a new instance for environment (EC2)`, earlier in this topic. Also, these commands assume
the latest stable version of Go at the time this topic was written; for more information, see `Downloads <https://golang.org/dl/>`_
on The Go Programming Language website.)

.. code-block:: sh

   wget https://storage.googleapis.com/golang/go1.9.3.linux-amd64.tar.gz # Download the Go installer.
   sudo tar -C /usr/local -xzf ./go1.9.3.linux-amd64.tar.gz              # Install Go.
   rm ./go1.9.3.linux-amd64.tar.gz                                       # Delete the Go installer, as you no longer need it.

After you install Go, add the path to the Go binary to your :code:`PATH` environment variable. To do this, add the following code
to the end of your shell profile file (for example, :file:`~/.bashrc` in Amazon Linux, assuming you
chose the option to :guilabel:`Create a new instance for environment (EC2)`, earlier in this topic), and then save the file.

.. code-block:: sh

    PATH=$PATH:/usr/local/go/bin

After you save the file, source the :file:`~/.bashrc` file so that the terminal can now find the Go binary you just referenced. To do this,
run the following command. (This command assumes you chose the option to :guilabel:`Create a new instance for environment (EC2)`,
earlier in this topic.)

.. code-block:: sh

   . ~/.bashrc

.. _cloud9-go-examples:

Step 4: Download Example Code
=============================

Use the terminal you opened in the previous step to download example code for the |sdk-go| into the |AC9| development environment.

To do this, run the following command. This command downloads a copy of all of the code examples
used in the official AWS SDK documentation into your environment's root directory.

.. code-block:: sh

   git clone https://github.com/awsdocs/aws-doc-sdk-examples.git

To find code examples for the |sdk-go|, use the :guilabel:`Environment` window to open the
:file:`ENVIRONMENT_NAME/aws-doc-sdk-examples/go/example_code` directory,
where :file:`ENVIRONMENT_NAME` is the name of your development environment.

To learn how to work with these and other code examples, see :doc:`common-examples`.

.. _cloud9-go-run:

Step 5: Run Example Code
========================

To run code in your |AC9| development environment, see
`Run Your Code <https://docs.aws.amazon.com/cloud9/latest/user-guide/build-run-debug.html#build-run-debug-run>`_ in the |AC9-ug|.
