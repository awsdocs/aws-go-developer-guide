# Running a Lambda Function<a name="lambda-go-example-run-function"></a>

The following example runs the Lambda function `MyGetitemsFunction` in the `us-west-2` region\. This Node\.js function returns a list of items from a database\. The input JSON looks like the following\.

```
{
   "SortBy": "name|time",
   "SortOrder": "ascending|descending",
   "Number": 50
}
```

Where:
+  `SortBy` is the criteria for sorting the results\. Our example uses `time`, which means the returned items are sorted in the order in which they were added to the database\.
+  `SortOrder` is the order of sorting\. Our example uses `descending`, which means the most\-recent item is last in the list\.
+  `Number` is the maximum number of items to retrieve \(the default is 50\)\. Our example uses `10`, which means get the 10 most\-recent items\.

The output JSON looks like the following when the function succeeds and two items are returned\.

```
{
   "statusCode": 200,
   "body": {
      "result": "success",
      "error": ""
      "data": [
         {
            "item": "item1"
         },
         {
            "item": "item2"
         }
      ]
   }
}
```

Where:
+  `statusCode`– An HTTP status code; `200` means the call was successful\.
+  `body`– The body of the returned JSON\.
+  `result`– The result of the call, either `success` or `failure`\.
+  `error`– An error message if `result` is `failure`; otherwise, an empty string\.
+  `data`– The returned results if `result` is `success`; otherwise, nil\.
+  `item`– An item from the list of results\.

The first step is to import the packages we use\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/lambda"
    
    "encoding/json"
    "fmt"
    "os"
    "strconv"
)
```

Next create session and Lambda client we use to invoke the Lambda function\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

client := lambda.New(sess, &aws.Config{Region: aws.String("us-west-2")})
```

Next, create the request and payload, and call `MyGetItemsFunction`\. If there is an error, display a message and quit\.

```
request := getItemsRequest{"time", "descending", 10}

payload, err := json.Marshal(request)
if err != nil {
    fmt.Println("Error marshalling MyGetItemsFunction request")
    os.Exit(0)
}

result, err := client.Invoke(&lambda.InvokeInput{FunctionName: aws.String("MyGetItemsFunction"), Payload: payload})
if err != nil {
    fmt.Println("Error calling MyGetItemsFunction")
    os.Exit(0)
}
```

Finally, parse the response, and if successful, print out the items\.

```
var resp getItemsResponse

err = json.Unmarshal(result.Payload, &resp)
if err != nil {
    fmt.Println("Error unmarshalling MyGetItemsFunction response")
    os.Exit(0)
}

// If the status code is NOT 200, the call failed
if resp.StatusCode != 200 {
    fmt.Println("Error getting items, StatusCode: " + strconv.Itoa(resp.StatusCode))
    os.Exit(0)
}

// If the result is failure, we got an error
if resp.Body.Result == "failure" {
    fmt.Println("Failed to get items")
    os.Exit(0)
}

// Print out items
if len(resp.Body.Data) > 0 {
    for i := range resp.Body.Data {
        fmt.Println(resp.Body.Data[i].Item)
    }
} else {
    fmt.Println("There were no items")
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/lambda/aws-go-sdk-lambda-example-run-function.go) on GitHub\.

**Note**  
The [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/lambda/aws-go-sdk-lambda-example-run-function.go) includes the structures for marshaling the JSON request and unmarshaling the JSON response\.