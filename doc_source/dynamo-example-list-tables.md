# Listing all Amazon DynamoDB Tables<a name="dynamo-example-list-tables"></a>

The following example uses the DynamoDB [ListTables](https://docs.aws.amazon.com/sdk-for-go/api/service/dynamodb/#DynamoDB.ListTables) operation to list all of the tables in your default region\.

Create the file *DynamoDBListTables\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws/awserr"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/dynamodb"

    "fmt"
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

Call **ListTables**\. If an error occurs, print the error and exit\. If no error occurs, loop through the tables, printing the name of each table\.

```
// create the input configuration instance
input := &dynamodb.ListTablesInput{}

fmt.Printf("Tables:\n")

for {
    // Get the list of tables
    result, err := svc.ListTables(input)
    if err != nil {
        if aerr, ok := err.(awserr.Error); ok {
            switch aerr.Code() {
            case dynamodb.ErrCodeInternalServerError:
                fmt.Println(dynamodb.ErrCodeInternalServerError, aerr.Error())
            default:
                fmt.Println(aerr.Error())
            }
        } else {
            // Print the error, cast err to awserr.Error to get the Code and
            // Message from an error.
            fmt.Println(err.Error())
        }
        return
    }

    for _, n := range result.TableNames {
        fmt.Println(*n)
    }

    // assign the last read tablename as the start for our next call to the ListTables function
    // the maximum number of table names returned in a call is 100 (default), which requires us to make
    // multiple calls to the ListTables function to retrieve all table names
    input.ExclusiveStartTableName = result.LastEvaluatedTableName

    if result.LastEvaluatedTableName == nil {
        break
    }
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/dynamodb/DynamoDBListTables.go) on GitHub\.