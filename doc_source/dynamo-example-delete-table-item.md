# Deleting an Amazon DynamoDB Table Item<a name="dynamo-example-delete-table-item"></a>

The following example uses the DynamoDB [DeleteItem](https://docs.aws.amazon.com/sdk-for-go/api/service/dynamodb/#DynamoDB.DeleteItem) operation to delete the item with the `year` **2015** and `title` **The Big New Movie** from the `Movies` table in your default region\.

Create the file *DynamoDBUpdateItem\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/dynamodb"

    "fmt"
    "log"
)
```

Initialize a session that the SDK will use to load credentials from the shared credentials file *\~/\.aws/credentials* and region from the shared configuration file *\~/\.aws/config* and create a new DynamoDB service client\.

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

Call **DeleteItem** to delete the item from the table\. If we encounter an error, print the error message\. Otherwise, display a message that the item was deleted\.

```
tableName := "Movies"
movieName := "The Big New Movie"
movieYear := "2015"

input := &dynamodb.DeleteItemInput{
    Key: map[string]*dynamodb.AttributeValue{
        "Year": {
            N: aws.String(movieYear),
        },
        "Title": {
            S: aws.String(movieName),
        },
    },
    TableName: aws.String(tableName),
}

_, err := svc.DeleteItem(input)
if err != nil {
    log.Fatalf("Got error calling DeleteItem: %s", err)
}

fmt.Println("Deleted '" + movieName + "' (" + movieYear + ") from table " + tableName)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/dynamodb/DynamoDBDeleteItem.go) on GitHub\.