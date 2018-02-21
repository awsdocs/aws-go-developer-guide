.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _kms-example-encrypt-data:

##############################
Encrypting Data with |KMSlong|
##############################

.. meta::
    :description:
        Encrypt data in AWS KMS using this AWS SDK for Go code example.
    :keywords: AWS SDK for Go code examples, KMS

The following example uses the |sdk-go|
`Encrypt <http://docs.aws.amazon.com/sdk-for-go/api/service/kms/#KMS.Encrypt>`_ method,
which implements the
`Encrypt <http://docs.aws.amazon.com/kms/latest/APIReference/API_Encrypt.html>`_ operation,
to encrypt the string "1234567890".
The example displays a readable version of the resulting encrypted blob.

.. literalinclude:: ./example_code/kms/kms_encrypt_data.go
   :lines: 17-57
   :dedent: 0
   :language: go

Choose :code:`Copy` to save the code locally.
See the `complete example
<https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/go/example_code/kms/kms_encrypt_data.go>`_
on GitHub.
