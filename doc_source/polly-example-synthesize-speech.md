# Synthesizing Speech<a name="polly-example-synthesize-speech"></a>

This example uses the [SynthesizeSpeech](https://docs.aws.amazon.com/sdk-for-go/api/service/polly/#Polly.SynthesizeSpeech) operation to get the text from a file and produce an MP3 file containing the synthesized speech\.

Choose `Copy` to save the code locally\.

Create the file *pollySynthesizeSpeech\.go*\. Import the packages used in the example\.

```
import (
    "github.com/aws/aws-sdk-go/aws"
    "github.com/aws/aws-sdk-go/aws/session"
    "github.com/aws/aws-sdk-go/service/polly"

    "fmt"
    "os"
    "strings"
    "io"
    "io/ioutil"
)
```

Get the name of the text file from the command line\.

```
if len(os.Args) != 2 {
    fmt.Println("You must supply an alarm name")
    os.Exit(1)
}

fileName := os.Args[1]
```

Open the text file and read the contents as a string\.

```
contents, err := ioutil.ReadFile(fileName)
s := string(contents[:])
```

Initialize a session that the SDK will use to load credentials from the shared credentials file `~/.aws/credentials`, load your configuration from the shared configuration file `~/.aws/config`, and create an Amazon Polly client\.

```
sess := session.Must(session.NewSessionWithOptions(session.Options{
    SharedConfigState: session.SharedConfigEnable,
}))

svc := polly.New(sess)
```

Create the input for and call `SynthesizeSpeech`\.

```
input := &polly.SynthesizeSpeechInput{OutputFormat: aws.String("mp3"), Text: aws.String(s), VoiceId: aws.String("Joanna")}

output, err := svc.SynthesizeSpeech(input)
```

Save the resulting synthesized speech as an MP3 file\.

```
names := strings.Split(fileName, ".")
name := names[0]
mp3File := name + ".mp3"

outFile, err := os.Create(mp3File)
defer outFile.Close()
```

**Note**  
The resulting MP3 file is in the MPEG\-2 format\.

See the [complete example](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/polly/pollySynthesizeSpeech.go) on GitHub\.