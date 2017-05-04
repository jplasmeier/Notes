# AWS Cryptography 101

[Notes start here.](http://docs.aws.amazon.com/kms/latest/developerguide/crypto-intro.html)

## AWS Key Management Service Overview

AWS KMS is a managed service that allows you to create and control the keys used to encrypt your data. Integrated into EBS, S3, RDS, etc. as well as CloudTrail for key usage logging. 

Management Options for Master Keys

* Create, describe, and list master keys
* Enable and disable master keys
* Set and retrieve master key usage policies (access control)
* Create, delete, list, and update aliases, which are friendly names that point to your master keys
* Delete master keys to complete the key lifecycle

Actions for using Master Keys

* Encrypt, decrypt, and re-encrypt data
* Generate data encryption keys that you can export from the service in plaintext or encrypted under a master key that doesn't leave the service
* Generate random numbers suitable for cryptographic applications

## Encryption Basics

AWS KMS uses AES-GCM with 256-bit keys. 

#### Symmetrics vs Asymmetric Encryption

Symmmetric encryption uses just one key for encryption and decryption. 

Assymetric encryption uses two keys, a public key and a private key. The public key is used to encrypt data and the private key is used to decrypt it. 

AWS KMS uses symmetric encryption.

### Symmetric Encryption

A single key and plaintext are fed into an encryption algorithm which outputs ciphertext. This process works in reverse as well; ciphertext plus the same key go through a decryption algorithm and plaintext comes out. 

### Authenticated Encryption

The `Encrypt` API takes:

* plaintext
* a customer master key (CMK) identifier
* an encryption context 

and returns ciphertext.

The encryption context represents AAD - additional authenticated data, which is used to generate an authentication tag included in the output of the ciphertext. You must call decrypt with the same encryption context in order to match the authentication tags. Furthermore, if the ciphertext has been modified, the algorithm will fail. 

### Encryption Context

Represented by a set of key-value pairs. Passed into encryption/decryption calls and checked for integrity. It is not stored in the ciphertext, but is cryptographically represented therein. 

It can actually be anything, but should not be senstive information since it is not encrypted. Suggested to use a description of the data being encrypted so CloudTrail logs are meaningful. 

Amazon EBS uses the ID of the encrypted volume as the encryption context for server-side encryption. If you're encrypting a file, consider using part of the filename. 

### Encryption Context in Grants and Key Policies

AWS KMS also supports authorization by grants and key policies that incorporate encryption context. Grants are effectively constraints on encryption context. In the above example of EBS, the volume ID is actually bound to the encryption context via `Constraint` passed into `CreateGrant`. This provides tighter, more granular access than just the CMK. 

### Storing Encryption Context

Usually, it's easiest to store the encryption context along with the ciphertext that you're decrypting/reencrypting. You can also make encryption context location-sensitive. In the above example of a filename, you could pass the full filepath as the encryption context but only store the filename alongside the ciphertext. Then, when you need to decrypt, use the full filepath. If the files have moved, the thief cannot use your encryption context as the filepath has changed. 

### Master Key

A key created by AWS KMS that can only be used within the AWS KMS service. The master key is commonly used to encrypt data keys so that the encrypted key can be securely stored by your service. However, AWS KMS master keys can also be used to encrypt or decrypt arbitrary chunks of data that are no greater than 4 KiB. Master keys are categorized as either customer managed keys or AWS managed keys. Customer managed keys are created by a customer for use by a service or application. AWS managed keys are the default keys used by AWS services that support encryption.

## AWS KMS Concepts 



## Glossary

|term|definition|
|----|----------|
|KMS |Key Management Service|
|CMK |Customer Master Key|


