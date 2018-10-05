AWS::AmazonMQ::Configuration

A configuration contains all of the settings for your ActiveMQ broker, in XML format.

The AWS::AmazonMQ::Configuration resource lets you create Amazon MQ configurations, add configuration changes or modify users, and return information about the specified configuration. For more information, see Configuration and Amazon MQ Broker Configuration Parameters in the Amazon MQ Developer Guide.

Topics

    Syntax
    Properties
    Return Values
    Examples

Syntax

To declare this entity in your AWS CloudFormation template, use the following syntax:
JSON

{
  "Type" : "AWS::AmazonMQ::Configuration",
  "Properties" : {
    "Data" : String,
    "Description" : String,
    "EngineType" : String,
    "EngineVersion" : String,
    "Name" : String
  }
}

YAML

Type: "AWS::AmazonMQ::Configuration"
Properties:
  Data: String
  Description: String
  EngineType: String
  EngineVersion: String
  Name: String

Properties

Data

    The base64-encoded XML configuration.

    Required: Yes

    Type: String

    Update requires: No interruption
Description

    The description of the configuration.

    Required: No

    Type: String

    Update requires: No interruption
EngineType

    The type of broker engine.

    Note

    Currently, Amazon MQ supports only ACTIVEMQ.

    Required: Yes

    Type: String

    Update requires: Replacement
EngineVersion

    The version of the broker engine.

    Note

    Currently, Amazon MQ supports only 5.15.0.

    Required: Yes

    Type: String

    Update requires: Replacement
Name

    The name of the configuration. This value can contain only alphanumeric characters, dashes, periods, underscores, and tildes (- . _ ~). This value must be 1-150 characters long.

    Required: Yes

    Type: String

    Update requires: Replacement

Return Values
Ref

When you pass the logical ID of an AWS::AmazonMQ::Configuration resource to the intrinsic Ref function, the function returns the Amazon MQ configuration ID. For example:

c-1234a5b6-78cd-901e-2fgh-3i45j6k178l9

For more information about using the Ref function, see Ref.
Fn::GetAtt

Fn::GetAtt returns a value for a specified attribute of this type. The following are the available attributes and sample return values.

Arn

    The Amazon Resource Name (ARN) of the Amazon MQ configuration.

    arn:aws:mq:us-east-2:123456789012:configuration:MyConfigurationDevelopment:c-1234a5b6-78cd-901e-2fgh-3i45j6k178l9

Revision

    The revision number of the configuration.

    1

For more information about using Fn::GetAtt, see Fn::GetAtt.
Examples
Amazon MQ Configuration

The following example creates an Amazon MQ configuration in XML format.
JSON

{
  "Description": "Create an Amazon MQ configuration",
    "Configuration1": {
      "Type": "AWS::AmazonMQ::Configuration",
      "Properties": {
        "Data": {
          "Fn::Base64": "<?xml version=\"1.0\" encoding=\"UTF-8\" standalone=\"yes\"?>\n<broker xmlns=\"http://activemq.apache.org/schema/core\" start=\"false\">\n  <destinationPolicy>\n    <policyMap>\n      <policyEntries>\n        <policyEntry topic=\">\">\n          <pendingMessageLimitStrategy>\n            <constantPendingMessageLimitStrategy limit=\"3000\"/>\n          </pendingMessageLimitStrategy>\n        </policyEntry>\n      </policyEntries>\n    </policyMap>\n  </destinationPolicy>\n  <plugins>\n  </plugins>\n</broker>\n"
        },
        "EngineType": "ACTIVEMQ",
        "EngineVersion": "5.15.0",
        "Name": "my-configuration-1"
      }
   }
}

YAML

--- 
Description: "Create an Amazon MQ configuration"
Resources: 
  Configuration: 
    Type: "AWS::AmazonMQ::Configuration"
    Properties: 
      Data: 
        ? "Fn::Base64"
        : |
            <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
            <broker xmlns="http://activemq.apache.org/schema/core" start="false">
              <destinationPolicy>
                <policyMap>
                  <policyEntries>
                    <policyEntry topic=">">
                      <pendingMessageLimitStrategy>
                        <constantPendingMessageLimitStrategy limit="3000"/>
                      </pendingMessageLimitStrategy>
                    </policyEntry>
                  </policyEntries>
                </policyMap>
              </destinationPolicy>
              <plugins>
              </plugins>
            </broker>
      EngineType: ACTIVEMQ
      EngineVersion: "5.15.0"
      Name: my-configuration-1

