# Using AWS Cloud9 with the AWS SDK for Go<a name="cloud9-go"></a>

You can use AWS Cloud9 with the AWS SDK for Go to write and run your Go code using just a browser\. AWS Cloud9 includes tools such as a code editor and terminal\. Because the AWS Cloud9 IDE is cloud based, you can work on your projects from your office, home, or anywhere using an internet\-connected machine\. For general information about AWS Cloud9, see the AWS Cloud9 User Guide\.

Follow these instructions to set up AWS Cloud9 with the AWS SDK for Go:
+  [Step 1: Set up Your AWS Account to Use AWS Cloud9](#cloud9-go-account) 
+  [Step 2: Set up Your AWS Cloud9 Development Environment](#cloud9-go-environment) 
+  [Step 3: Set up the AWS SDK for Go](#cloud9-go-sdk) 
+  [Step 4: Download Example Code](#cloud9-go-examples) 
+  [Step 5: Run Example Code](#cloud9-go-run) 

## Step 1: Set up Your AWS Account to Use AWS Cloud9<a name="cloud9-go-account"></a>

Start to use AWS Cloud9 by signing in to the AWS Cloud9 console as an AWS Identity and Access Management \(IAM\) entity \(for example, an IAM user\) in your AWS account who has access permissions for AWS Cloud9\.

To set up an IAM entity in your AWS account to access AWS Cloud9, and to sign in to the AWS Cloud9 console, see [Team Setup for AWS Cloud9](https://docs.aws.amazon.com/cloud9/latest/user-guide/setup.html) in the AWS Cloud9 User Guide\.

## Step 2: Set up Your AWS Cloud9 Development Environment<a name="cloud9-go-environment"></a>

After you sign in to the AWS Cloud9 console, use the console to create an AWS Cloud9 development environment\. After you create the environment, AWS Cloud9 opens the IDE for that environment\.

See [Creating an Environment in AWS Cloud9](https://docs.aws.amazon.com/cloud9/latest/user-guide/create-environment.html) in the AWS Cloud9 User Guide for details\.

**Note**  
As you create your environment in the console for the first time, we recommend that you choose the option to **Create a new instance for environment \(EC2\)**\. This option tells AWS Cloud9 to create an environment, launch an Amazon EC2 instance, and then connect the new instance to the new environment\. This is the fastest way to begin using AWS Cloud9\.

## Step 3: Set up the AWS SDK for Go<a name="cloud9-go-sdk"></a>

After AWS Cloud9 opens the IDE for your development environment, use the IDE to set up the AWS SDK for Go in your environment, as follows\.

1. If the terminal isn’t already open in the IDE, open it\. On the menu bar in the IDE, choose **Window, New Terminal**\.

1. Set your GOPATH environment variable\. To do this, add the following code to the end of your shell profile file \(for example, `~/.bashrc` in Amazon Linux, assuming you chose the option to **Create a new instance for environment \(EC2\)**, earlier in this topic\), and then save the file\.

   ```
   GOPATH=~/environment/go
   
   export GOPATH
   ```

   After you save the file, source the `~/.bashrc` file to finish setting your GOPATH environment variable\. To do this, run the following command\. \(This command assumes you chose the option to **Create a new instance for environment \(EC2\)**, earlier in this topic\.\)

   ```
   . ~/.bashrc
   ```

1. Run the following command to install the AWS SDK for Go\.

   ```
   go get -u github.com/aws/aws-sdk-go/...
   ```

If the IDE can’t find Go, run the following commands, one at a time in this order, to install it\. \(These commands assume you chose the option to **Create a new instance for environment \(EC2\)**, earlier in this topic\. Also, these commands assume the latest stable version of Go at the time this topic was written; for more information, see [Downloads](https://golang.org/dl/) on The Go Programming Language website\.\)

```
wget https://storage.googleapis.com/golang/go1.9.3.linux-amd64.tar.gz # Download the Go installer.
sudo tar -C /usr/local -xzf ./go1.9.3.linux-amd64.tar.gz              # Install Go.
rm ./go1.9.3.linux-amd64.tar.gz                                       # Delete the Go installer, as you no longer need it.
```

After you install Go, add the path to the Go binary to your `PATH` environment variable\. To do this, add the following code to the end of your shell profile file \(for example, `~/.bashrc` in Amazon Linux, assuming you chose the option to **Create a new instance for environment \(EC2\)**, earlier in this topic\), and then save the file\.

```
PATH=$PATH:/usr/local/go/bin
```

After you save the file, source the `~/.bashrc` file so that the terminal can now find the Go binary you just referenced\. To do this, run the following command\. \(This command assumes you chose the option to **Create a new instance for environment \(EC2\)**, earlier in this topic\.\)

```
. ~/.bashrc
```

## Step 4: Download Example Code<a name="cloud9-go-examples"></a>

Use the terminal you opened in the previous step to download example code for the AWS SDK for Go into the AWS Cloud9 development environment\.

To do this, run the following command\. This command downloads a copy of all of the code examples used in the official AWS SDK documentation into your environment’s root directory\.

```
git clone https://github.com/awsdocs/aws-doc-sdk-examples.git
```

To find code examples for the AWS SDK for Go, use the **Environment** window to open the `ENVIRONMENT_NAME/aws-doc-sdk-examples/go/example_code` directory, where `ENVIRONMENT_NAME` is the name of your development environment\.

To learn how to work with these and other code examples, see [AWS SDK for Go Code Examples](common-examples.md)\.

## Step 5: Run Example Code<a name="cloud9-go-run"></a>

To run code in your AWS Cloud9 development environment, see [Run Your Code](https://docs.aws.amazon.com/cloud9/latest/user-guide/build-run-debug.html#build-run-debug-run) in the AWS Cloud9 User Guide\.