.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _polly-example-synthesize-speech:

###################
Synthesizing Speech
###################

.. meta::
    :description:
        Synthesizing speech using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, Amazon Polly

This example uses the
:sdk-go-api-deep:`SynthesizeSpeech <service/polly/#Polly.SynthesizeSpeech>` operation
to get the text from a file and produce an MP3 file containing the synthesized speech.

Choose :code:`Copy` to save the code locally.

Create the file *pollySynthesizeSpeech.go*.
Import the packages used in the example.

.. literalinclude:: ./example_code/polly/pollySynthesizeSpeech.go
   :lines: 17-27
   :dedent: 0
   :language: go

Get the name of the text file from the command line.

.. literalinclude:: ./example_code/polly/pollySynthesizeSpeech.go
   :lines: 30-34, 36
   :dedent: 4
   :language: go

Open the text file and read the contents as a string.

.. literalinclude:: ./example_code/polly/pollySynthesizeSpeech.go
   :lines: 39, 47
   :dedent: 4
   :language: go

Initialize a session that the SDK will use to load credentials
from the shared credentials file :file:`~/.aws/credentials`,
load your configuration from the shared configuration file :file:`~/.aws/config`,
and create an Amazon Polly client.

.. literalinclude:: ./example_code/polly/pollySynthesizeSpeech.go
   :lines: 51-54, 56
   :dedent: 4
   :language: go

Create the input for and call :code:`SynthesizeSpeech`.

.. literalinclude:: ./example_code/polly/pollySynthesizeSpeech.go
   :lines: 59-61
   :dedent: 4
   :language: go

Save the resulting synthesized speech as an MP3 file.

.. literalinclude:: ./example_code/polly/pollySynthesizeSpeech.go
   :lines: 69-73, 80
   :dedent: 4
   :language: go

.. note:: The resulting MP3 file is in the MPEG-2 format.

See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/polly/pollySynthesizeSpeech.go>`_
on GitHub.
