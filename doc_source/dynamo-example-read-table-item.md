# Reading an Amazon DynamoDB Table Item<a name="dynamo-example-read-table-item"></a>

The following example uses the DynamoDB [GetItem](https://docs.aws.amazon.com/sdk-for-go/api/service/dynamodb/#DynamoDB.GetItem) operation to retrieve information about the item with the `year` **2015** and `title` **The Big New Movie** in the `movies` table in your default region\.

Create the file *DynamoDBReadItem\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/dynamodb"
    "github.com/aws/aws-sdk-go/service/dynamodb/dynamodbattribute"

    "fmt"
    "log"
)
```

Create the data structure we use to contain the information about the table item\.

```
// Create struct to hold info about new item
type Item struct {
    Year   int
    Title  string
    Plot   string
    Rating float64
}
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

Call **GetItem** to get the item from the table\. If we encounter an error, print the error message\. Otherwise, display information about the item\.

```
tableName := "Movies"
movieName := "The Big New Movie"
movieYear := "2015"

result, err := svc.GetItem(&dynamodb.GetItemInput{
    TableName: aws.String(tableName),
    Key: map[string]*dynamodb.AttributeValue{
        "Year": {
            N: aws.String(movieYear),
        },
        "Title": {
            S: aws.String(movieName),
        },
    },
})
if err != nil {
    log.Fatalf("Got error calling GetItem: %s", err)
}
```

If an item was returned, unmarshall the return value and display the year, title, plot, and rating\.

```
if result.Item == nil {
    msg := "Could not find '" + *title + "'"
    return nil, errors.New(msg)
}
    
item := Item{}

err = dynamodbattribute.UnmarshalMap(result.Item, &item)
if err != nil {
    panic(fmt.Sprintf("Failed to unmarshal Record, %v", err))
}

fmt.Println("Found item:")
fmt.Println("Year:  ", item.Year)
fmt.Println("Title: ", item.Title)
fmt.Println("Plot:  ", item.Plot)
fmt.Println("Rating:", item.Rating)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/dynamodb/DynamoDBReadItem.go) on GitHub\.