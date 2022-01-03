# Creating an Amazon DynamoDB Table Item<a name="dynamo-example-create-table-item"></a>

The following example uses the DynamoDB [PutItem](https://docs.aws.amazon.com/sdk-for-go/api/service/dynamodb/#DynamoDB.PutItem) operation to create the table item with the `year` **2015** and `title` **The Big New Movie** in the `Movies` table in your default region\.

Create the file *DynamoDBCreateItem\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/dynamodb"
    "github.com/aws/aws-sdk-go/service/dynamodb/dynamodbattribute"

    "fmt"
    "log"
    "strconv"
)
```

Create the data structure we use to containing the information about the table item\.

```
// Create struct to hold info about new item
type Item struct {
    Year   int
    Title  string
    Plot   string
    Rating float64
}
```

Initialize a session that the SDK will use to load credentials from the shared credentials file *\~/\.aws/credentials* and region from the shared configuration file *\~/\.aws/config*, and create the DynamoDB client\.

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

Create a struct with the movie data and marshall that data into a map of **AttributeValue** objects\.

```
item := Item{
    Year:   2015,
    Title:  "The Big New Movie",
    Plot:   "Nothing happens at all.",
    Rating: 0.0,
}

av, err := dynamodbattribute.MarshalMap(item)
if err != nil {
    log.Fatalf("Got error marshalling new movie item: %s", err)
}
```

Create the input for **PutItem** and call it\. If an error occurs, print the error and exit\. If no error occurs, print an message that the item was added to the table\.

```
// Create item in table Movies
tableName := "Movies"

input := &dynamodb.PutItemInput{
    Item:      av,
    TableName: aws.String(tableName),
}

_, err = svc.PutItem(input)
if err != nil {
    log.Fatalf("Got error calling PutItem: %s", err)
}

year := strconv.Itoa(item.Year)

fmt.Println("Successfully added '" + item.Title + "' (" + year + ") to table " + tableName)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/dynamodb/DynamoDBCreateItem.go) on GitHub\.