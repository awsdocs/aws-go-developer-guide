# Set up SDK Metrics in the AWS SDK for Go<a name="setup-metrics"></a>

The following steps demonstrate how to set up SDK Metrics for the AWS SDK for Go\. These steps pertain to an Amazon EC2 instance running Amazon Linux for a client application that is using the AWS SDK for Go\. SDK Metrics is also available for your production environments if you enable it while configuring the AWS SDK for Go\.

To use SDK Metrics, run the latest version of the CloudWatch agent\.

For details about IAM Permissions for SDK Metrics, see [Authorize SDK Metrics to Collect and Send Metrics in the AWS SDK for Go](authorize-metrics.md)\.

To set up SDK Metrics with the AWS SDK for Go:

1. Create an application with an AWS SDK for Go client to use an AWS service\.

1. Host your project on an Amazon EC2 instance or in your local environment\.

1. Install and use the latest version of the AWS SDK for Go\.

1. Install and configure a CloudWatch agent on an Amazon EC2 instance or in your local environment\.

1. Authorize SDK Metrics to collect and send metrics\.

1.  [Enable SDK Metrics for the AWS SDK for Go](#enable-sdk-metrics)\.

For more information, see:
+  [Update a CloudWatch Agent](#update-cw-agent)\.
+  [Disable SDK Metrics](#disable-sdk-metrics)\.

## Enable SDK Metrics for the AWS SDK for Go<a name="enable-sdk-metrics"></a>

By default, SDK Metrics is turned off, and the port is set to 31000\. The following are the default parameters\.

```
//default values
[
  'enabled' => false,
  'port' => 31000,
]
```

Enabling SDK Metrics is independent of configuring your credentials to use an AWS service\.

You can enable SDK Metrics by setting environment variables or by using the shared AWS `config` file\.

### Option 1: Set Environment Variables<a name="enable-csm-option-1"></a>

The SDK first checks the profile specified in the environment variable under `AWS_PROFILE` to determine if SDK Metrics is enabled\.

To turn on SDK Metrics, add the following to your environmental variables\.

 `export AWS_CSM_ENABLED=true` 

Other configuration settings are available, see [Update a CloudWatch Agent](#update-cw-agent) for details\. For more information about using shared files, see the environment variables information in [Configuring the AWS SDK for Go](configuring-sdk.md)\.

**Note**  
Enabling SDK Metrics does not configure your credentials to use an AWS service\. To do that, see [Specifying Credentials](configuring-sdk.md#specifying-credentials)\.

### Option 2: Shared AWS`config` file<a name="enable-csm-option-2"></a>

If no SDK Metrics configuration is found in the environment variables, the AWS SDK for Go looks for your customized AWS profile field\. Then it checks the `aws_csm` profile\. To enable SDK Metrics, add `csm_enabled` to the shared config file `~/.aws/config`\.

```
[default]
csm_enabled = true

[profile aws_csm]
csm_enabled = true
```

Other configuration settings are available, see [Update a CloudWatch Agent](#update-cw-agent) for details\. For more information about using shared files, see the environment variables information in [Configuring the AWS SDK for Go](configuring-sdk.md)\.

**Note**  
Enabling SDK Metrics does not configure your credentials to use an AWS service\. To do that, see [Specifying Credentials](configuring-sdk.md#specifying-credentials)\.

## Update a CloudWatch Agent<a name="update-cw-agent"></a>

To make changes to the port number or the Client ID, set the values and then restart any AWS jobs that are currently active\.

### Option 1: Set Environment Variables<a name="update-cw-agent-option1"></a>

Most AWS services use the default port\. But if the service you want SDK Metrics to monitor uses a unique port, add `AWS_CSM_PORT=[PORT-NUMBER]`, where PORT\-NUMBER is the port number, to the host’s environment variables\.

```
export AWS_CSM_ENABLED=true
export AWS_CSM_PORT=1234
export AWS_CSM_CLIENT_ID="My Application"
```

### Option 2: Shared AWS`config` file<a name="update-cw-agent-option2"></a>

Most services use the default port\. If your service requires a unique port ID, add `AWS_CSM_PORT=[PORT-NUMBER]`, where PORT\-NUMBER is the port number, to `~/.aws/config`\.

```
[default]
csm_enabled = false
csm_port = 1234
csm_client_id = "My Application"

[profile aws_csm]
csm_enabled = false
csm_port = 1234
csm_client_id = "My Application"
```

### Restart SDK Metrics<a name="restart-csm"></a>

To restart a job, run the following commands\.

```
amazon-cloudwatch-agent-ctl -a stop;
amazon-cloudwatch-agent-ctl -a start;
```

## Disable SDK Metrics<a name="disable-sdk-metrics"></a>

To turn off SDK Metrics, set `csm_enabled` to **false** in your environment variables or in your shared AWS `config` file `~/.aws/config`\. Then restart your CloudWatch agent so that the changes can take effect\.

### Set `csm_enabled` to **false**<a name="set-csm-enabled-false"></a>

#### Option 1: Environment Variables<a name="set-csm-enabled-false-option1"></a>

 `export AWS_CSM_ENABLED=false` 

#### Option 2: Shared AWS`config` file<a name="id4"></a>

**Note**  
Environment variables override the shared AWS `config` file\. If SDK Metrics is enabled in the environment variables, the SDK Metrics remains enabled\.

```
[default]
csm_enabled = false

[profile aws_csm]
csm_enabled = false
```

### Stop SDK Metrics and Restart CloudWatch Agent<a name="stop-csm-restart-cw-agent"></a>

To disable SDK Metrics, use the following command\.

 `sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a stop && echo "Done"` 

If you are using other CloudWatch features, restart CloudWatch with the following command\.

 `amazon-cloudwatch-agent-ctl -a start;` 