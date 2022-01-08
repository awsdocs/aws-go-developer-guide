# Creating an Amazon DynamoDB Table<a name="dynamo-example-create-table"></a>

The following example uses the DynamoDB [CreateTable](https://docs.aws.amazon.com/sdk-for-go/api/service/dynamodb/#DynamoDB.CreateTable) operation to create the table **Music** your default region\.

Create the file *DynamoDBCreateTable\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/dynamodb"

    "fmt"
    "log"
)
```

Initialize a session that the SDK will use to load credentials from the shared credentials file `~/.aws/credentials` and region from the shared configuration file `~/.aws/config`, and create the DynamoDB client\.

```
// Initialize a session that the SDK will use to load
// credentials from the shared credentials file ~/.aws/credentials
// and region from the shared configuration file ~/.aws/config.
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

// Create DynamoDB client
svc := dynamodb.New(sess)
```

Call **CreateTable**\. If an error occurs, print the error and exit\. If no error occurs, print an message that the table was created\.

```
// Create table Movies
tableName := "Movies"

input := &dynamodb.CreateTableInput{
    AttributeDefinitions: []*dynamodb.AttributeDefinition{
        {
            AttributeName: aws.String("Year"),
            AttributeType: aws.String("N"),
        },
        {
            AttributeName: aws.String("Title"),
            AttributeType: aws.String("S"),
        },
    },
    KeySchema: []*dynamodb.KeySchemaElement{
        {
            AttributeName: aws.String("Year"),
            KeyType:       aws.String("HASH"),
        },
        {
            AttributeName: aws.String("Title"),
            KeyType:       aws.String("RANGE"),
        },
    },
    ProvisionedThroughput: &dynamodb.ProvisionedThroughput{
        ReadCapacityUnits:  aws.Int64(10),
        WriteCapacityUnits: aws.Int64(10),
    },
    TableName: aws.String(tableName),
}

_, err := svc.CreateTable(input)
if err != nil {
    log.Fatalf("Got error calling CreateTable: %s", err)
}

fmt.Println("Created the table", tableName)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/dynamodb/DynamoDBCreateTable.go) on GitHub\.