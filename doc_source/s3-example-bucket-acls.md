# Working with Amazon S3 Bucket ACLs<a name="s3-example-bucket-acls"></a>

The following examples use AWS SDK for Go functions to:
+ Get the access control lists \(ACLs\) on a bucket
+ Get the ACLs on a bucket item
+ Add a new user to the ACLs on a bucket
+ Add a new user to the ACLs on a bucket item

You can download complete versions of these example files from the [aws\-doc\-sdk\-examples](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/go/example_code/s3) repository on GitHub\.

## Scenario<a name="s3-examples-bucket-acls-scenario"></a>

In these examples, a series of Go routines are used to manage ACLs on your Amazon S3 buckets\. The routines use the AWS SDK for Go to perform Amazon S3 bucket operations using the following methods of the Amazon S3 client class:
+  [GetBucketAcl](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.GetBucketAcl) 
+  [GetObjectAcl](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.GetObjectAcl) 
+  [PutBucketAcl](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutBucketAcl) 
+  [PutObjectAcl](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutObjectAcl) 

## Prerequisites<a name="s3-examples-bucket-acls-prerequisites"></a>
+ You have [set up](setting-up.md), and [configured](configuring-sdk.md) the AWS SDK for Go\.
+ You are familiar with Amazon S3 bucket ACLs\. To learn more, see [Managing Access with ACLs](https://docs.aws.amazon.com/AmazonS3/latest/dev/S3_ACLs_UsingACLs.html) in the Amazon S3 Developer Guide\.

## Get a Bucket ACL<a name="s3-examples-bucket-acls-get-bucket-acl"></a>

The [GetBucketAcl](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.GetBucketAcl) function gets the ACLs on a bucket\.

The following example gets the ACLs on a bucket with the name specified as a command line argument\.

Create the file *s3\_get\_bucket\_acl\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    
    "fmt"
    "os"
)
```

Create a function to display errors and exit\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

This example requires one input parameter, the name of the bucket\. If the name is not supplied, call the error function and exit\.

```
if len(os.Args) != 2 {
    exitErrorf("Bucket name required\nUsage: go run", os.Args[0], "BUCKET")
}

bucket := os.Args[1]
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `~/.aws/credentials`, the region from the shared configuration file `~/.aws/config`, and create a new Amazon S3 service client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

// Create S3 service client
svc := s3.New(sess)
```

Call [GetBucketAcl](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.GetBucketAcl), passing in the name of the bucket\. If an error occurs, call `exitErrorf`\. If no error occurs, loop through the results and print out the name, type, and permission for the grantees\.

```
result, err := svc.GetBucketAcl(&s3.GetBucketAclInput{Bucket: &bucket})
if err != nil {
    exitErrorf(err.Error())
}

fmt.Println("Owner:", *result.Owner.DisplayName)
fmt.Println("")
fmt.Println("Grants")

for _, g := range result.Grants {
    // If we add a canned ACL, the name is nil
    if g.Grantee.DisplayName == nil {
        fmt.Println("  Grantee:    EVERYONE")
    } else {
        fmt.Println("  Grantee:   ", *g.Grantee.DisplayName)
    }

    fmt.Println("  Type:      ", *g.Grantee.Type)
    fmt.Println("  Permission:", *g.Permission)
    fmt.Println("")
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_get_bucket_acl.go) on GitHub\.

## Set a Bucket ACL<a name="s3-examples-bucket-acls-set-bucket-acl"></a>

The [PutBucketAcl](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutBucketAcl) function sets the ACLs on a bucket\.

The following example gives a user access by email address to a bucket with the bucket name and email address specified as command line arguments\. The user can also supply a permission argument\. However, if it isn’o’t supplied, the user is given READ access to the bucket\.

Create the file *s3\_put\_bucket\_acl\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    
    "fmt"
    "os"
)
```

Create a function to display errors and exit\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

Get the two required input parameters\. If the optional permission parameter is supplied, make sure it is one of the allowed values\. If not, print an error message and quit\.

```
if len(os.Args) < 3 {
    exitErrorf("Bucket name and email address required; permission optional (READ if omitted)\nUsage: go run", os.Args[0], "BUCKET EMAIL [PERMISSION]")
}

bucket := os.Args[1]
address := os.Args[2]

permission := "READ"

if len(os.Args) == 4 {
    permission = os.Args[3]

    if !(permission == "FULL_CONTROL" || permission == "WRITE" || permission == "WRITE_ACP" || permission == "READ" || permission == "READ_ACP") {
        fmt.Println("Illegal permission value. It must be one of:")
        fmt.Println("FULL_CONTROL, WRITE, WRITE_ACP, READ, or READ_ACP")
        os.Exit(1)

    }
}
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `~/.aws/credentials`, the region from the shared configuration file `~/.aws/config`, and create a new Amazon S3 service client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

// Create S3 service client
svc := s3.New(sess)
```

Get the existing ACLs and append the new user to the list\. If there is an error while retrieving the list, print an error message and quit\.

```
result, err := svc.GetBucketAcl(&s3.GetBucketAclInput{Bucket: &bucket})
if err != nil {
    exitErrorf(err.Error())
}

owner := *result.Owner.DisplayName
ownerId := *result.Owner.ID

// Existing grants
grants := result.Grants

// Create new grantee to add to grants
var newGrantee = s3.Grantee{EmailAddress: &address, Type: &userType}
var newGrant = s3.Grant{Grantee: &newGrantee, Permission: &permission}

// Add them to the grants
grants = append(grants, &newGrant)
```

Build the parameter list for the call based on the existing ACLs and the new user information\.

```
params := &s3.PutBucketAclInput{
    Bucket: &bucket,
    AccessControlPolicy: &s3.AccessControlPolicy{
        Grants: grants,
        Owner: &s3.Owner{
            DisplayName: &owner,
            ID:          &ownerId,
        },
    },
}
```

Call [PutBucketAcl](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutBucketAcl), passing in the parameter list\. If an error occurs, display a message and quit\. Otherwise, display a message indicating success\.

```
_, err = svc.PutBucketAcl(params)
if err != nil {
    exitErrorf(err.Error())
}

fmt.Println("Congratulations. You gave user with email address", address, permission, "permission to bucket", bucket)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_put_bucket_acl.go) on GitHub\.

## Making a Bucket Public using a Canned ACL<a name="s3-examples-bucket-acls-make-bucket-public-acl"></a>

As in the previous example, the [PutBucketAcl](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutBucketAcl) function sets the ACLs on a bucket\.

The following example gives everyone read\-only access to the items in the bucket with the bucket name specified as a command line argument\.

Create the file *s3\_make\_bucket\_public\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"

    "fmt"
    "os"
)
```

Create a function to display errors and exit\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

Get the required input parameter and create the **acl**\.

```
if len(os.Args) < 2 {
    exitErrorf("Bucket name required.\nUsage: go run", os.Args[0], "BUCKET")
}

bucket := os.Args[1]
acl := "public-read"
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `~/.aws/credentials` the region from the shared configuration file `~/.aws/config`, and create a new Amazon S3 service client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))
svc := s3.New(sess)
```

Create the input for and call [PutBucketAcl](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutBucketAcl)\. If an error occurs, display a message and quit\. Otherwise, display a message indicating success\.

```
params := &s3.PutBucketAclInput{
    Bucket: &bucket,
    ACL: &acl,
}

// Set bucket ACL
_, err := svc.PutBucketAcl(params)
if err != nil {
    exitErrorf(err.Error())
}

fmt.Println("Bucket " + bucket + " is now public")
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_make_bucket_public.go) on GitHub\.

## Get a Bucket Object ACL<a name="s3-examples-bucket-acls-get-bucket-object-acl"></a>

The [PutObjectAcl](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutObjectAcl) function sets the ACLs on a bucket item\.

The following example gets the ACLs for a bucket item with the bucket and item name specified as command line arguments\.

Create the file *s3\_get\_bucket\_object\_acl\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    
    "fmt"
    "os"
)
```

Create a function to display errors and exit\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

This example requires two input parameters, the names of the bucket and object\. If either name is not supplied, call the error function and exit\.

```
if len(os.Args) != 3 {
    exitErrorf("Bucket and object names required\nUsage: go run", os.Args[0], "BUCKET OBJECT")
}

bucket := os.Args[1]
key := os.Args[2]
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `~/.aws/credentials`, the region from the shared configuration file `~/.aws/config`, and create a new Amazon S3 service client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

// Create S3 service client
svc := s3.New(sess)
```

Call [GetObjectAcl](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.GetObjectAcl), passing in the names of the bucket and object\. If an error occurs, call `exitErrorf`\. If no error occurs, loop through the results and print out the name, type, and permission for the grantees\.

```
result, err := svc.GetObjectAcl(&s3.GetObjectAclInput{Bucket: &bucket, Key: &key})
if err != nil {
    exitErrorf(err.Error())
}

fmt.Println("Owner:", *result.Owner.DisplayName)
fmt.Println("")
fmt.Println("Grants")

for _, g := range result.Grants {
    fmt.Println("  Grantee:   ", *g.Grantee.DisplayName)
    fmt.Println("  Type:      ", *g.Grantee.Type)
    fmt.Println("  Permission:", *g.Permission)
    fmt.Println("")
}
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_get_bucket_object_acl.go) on GitHub\.

## Set a Bucket Object ACL<a name="s3-examples-bucket-acls-set-bucket-object-acl"></a>

The [PutObjectAcl](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutObjectAcl) function sets the ACLs on a bucket item\.

The following example gives a user access by email address to a bucket item, with the bucket and item names and email address specified as command line arguments\. The user can also supply a permission argument\. However, if it isn’t supplied, the user is given READ access to the bucket\.

Create the file *s3\_put\_bucket\_object\_acl\.go*\. Add the following statements to import the Go and AWS SDK for Go packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/s3"
    
    "fmt"
    "os"
)
```

Create a function to display errors and exit\.

```
func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

Get the three required input parameters\. If the optional permission parameter is supplied, make sure it is one of the allowed values\. If not, print an error message and quit\.

```
if len(os.Args) < 4 {
    exitErrorf("Bucket name, object name, and email address required; permission optional (READ if omitted)\nUsage: go run", os.Args[0], "BUCKET OBJECT EMAIL [PERMISSION]")
}

bucket := os.Args[1]
key := os.Args[2]
address := os.Args[3]

permission := "READ"

if len(os.Args) == 5 {
    permission = os.Args[4]

    if !(permission == "FULL_CONTROL" || permission == "WRITE" || permission == "WRITE_ACP" || permission == "READ" || permission == "READ_ACP") {
        fmt.Println("Illegal permission value. It must be one of:")
        fmt.Println("FULL_CONTROL, WRITE, WRITE_ACP, READ, or READ_ACP")
        os.Exit(1)
    }
}
```

Initialize the session that the SDK uses to load credentials from the shared credentials file `~/.aws/credentials`, and create a new Amazon S3 service client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

// Create S3 service client
svc := s3.New(sess)
```

Build the parameter list for the call based on the existing ACLs and the new user information\.

```
result, err := svc.GetObjectAcl(&s3.GetObjectAclInput{Bucket: &bucket, Key: &key})
if err != nil {
    exitErrorf(err.Error())
}

owner := *result.Owner.DisplayName
ownerId := *result.Owner.ID

// Existing grants
grants := result.Grants

// Create new grantee to add to grants
userType := "AmazonCustomerByEmail"
var newGrantee = s3.Grantee{EmailAddress: &address, Type: &userType}
var newGrant = s3.Grant{Grantee: &newGrantee, Permission: &permission}

// Add them to the grants
grants = append(grants, &newGrant)

params := &s3.PutObjectAclInput{
    Bucket: &bucket,
    Key:    &key,
    AccessControlPolicy: &s3.AccessControlPolicy{
        Grants: grants,
        Owner: &s3.Owner{
            DisplayName: &owner,
            ID:          &ownerId,
        },
    },
}
```

Call [PutObjectAcl](https://docs.aws.amazon.com/sdk-for-go/api/service/s3/#S3.PutObjectAcl), passing in the parameter list\. If an error occurs, display a message and quit\. Otherwise, display a message indicating success\.

```
_, err = svc.PutObjectAcl(params)
if err != nil {
    exitErrorf(err.Error())
}

fmt.Println("Congratulations. You gave user with email address", address, permission, "permission to bucket", bucket, "object", key)
```

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/go/example_code/s3/s3_put_bucket_object_acl.go) on GitHub\.