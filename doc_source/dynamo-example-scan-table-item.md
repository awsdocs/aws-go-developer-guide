# Getting Amazon DynamoDB Table Items Using Expression Builder<a name="dynamo-example-scan-table-item"></a>

The following example uses the DynamoDB [Scan](https://docs.aws.amazon.com/sdk-for-go/api/service/dynamodb/#DynamoDB.Scan) operation to get items with a `rating` greater than **4\.0** in the `year` **2013** in the `Movies` table in your default region\.

The example uses the [Expression Builder](http://aws.amazon.com/blogs/developer/introducing-amazon-dynamodb-expression-builder-in-the-aws-sdk-for-go/) package released in version 1\.11\.0 of the AWS SDK for Go in September 2017\.

Create the file *DynamoDBScanItem\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/dynamodb"
    "github.com/aws/aws-sdk-go/service/dynamodb/dynamodbattribute"
    "github.com/aws/aws-sdk-go/service/dynamodb/expression"

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

Create variables for the minimum rating and year for the table items to retrieve\.

```
tableName := "Movies"
minRating := 4.0
year := 2013
```

Create the expression defining the year for which we filter the table items to retrieve, and the projection so we get the title, year, and rating for each retrieved item\. Then build the expression\.

```
// Create the Expression to fill the input struct with.
// Get all movies in that year; we'll pull out those with a higher rating later
filt := expression.Name("Year").Equal(expression.Value(year))

// Or we could get by ratings and pull out those with the right year later
//    filt := expression.Name("info.rating").GreaterThan(expression.Value(min_rating))

// Get back the title, year, and rating
proj := expression.NamesList(expression.Name("Title"), expression.Name("Year"), expression.Name("Rating"))

expr, err := expression.NewBuilder().WithFilter(filt).WithProjection(proj).Build()
if err != nil {
    log.Fatalf("Got error building expression: %s", err)
}
```

Create the inputs for and call **Scan** to retrieve the items from the table \(the movies made in 2013\)\.

```
// Build the query input parameters
params := &dynamodb.ScanInput{
    ExpressionAttributeNames:  expr.Names(),
    ExpressionAttributeValues: expr.Values(),
    FilterExpression:          expr.Filter(),
    ProjectionExpression:      expr.Projection(),
    TableName:                 aws.String(tableName),
}

// Make the DynamoDB Query API call
result, err := svc.Scan(params)
if err != nil {
    log.Fatalf("Query API call failed: %s", err)
}
```

Loop through the movies from 2013 and display the title and rating for those where the rating is greater than 4\.0

```
numItems := 0

for _, i := range result.Items {
    item := Item{}

    err = dynamodbattribute.UnmarshalMap(i, &item)

    if err != nil {
        log.Fatalf("Got error unmarshalling: %s", err)
    }

    // Which ones had a higher rating than minimum?
    if item.Rating > minRating {
        // Or it we had filtered by rating previously:
        //   if item.Year == year {
        numItems++

        fmt.Println("Title: ", item.Title)
        fmt.Println("Rating:", item.Rating)
        fmt.Println()
    }
}

fmt.Println("Found", numItems, "movie(s) with a rating above", minRating, "in", year)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/dynamodb/DynamoDBScanItems.go) on GitHub\.