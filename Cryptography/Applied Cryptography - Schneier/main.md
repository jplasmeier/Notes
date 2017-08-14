# Applied Cryptography

Chapters with an asterisks are considered essential. 

### Chapter 1: Foundations*

#### 1.1: Terminology

Authentication: The receiver of a message can reliably verify the origin of the message.

Integrity: The receiver can verify that the contents of an encrypted message have not been altered. 

Nonrepudiation: A sender cannot falsely deny that they have sent a message. 

Cryptanalysis: The practice of recovering plaintext without direct access to the key. Usually it is assumed that attacker possesses the full specification of the cryptographic algorithm used. 

##### Types of Cryptanalytic Attacks

**Ciphertext-only**

Given ciphertext to use.

**Known-plaintext**

Given some plaintext and the ciphertext thereof to use.

**Chosen-plaintext**

Given a choice of plaintext and the ciphertext thereof to use.

Relies on access to an encryption function. 

**Adaptive Chosen-plaintext**

Same as chosen-plaintext, but emphasis on being able to choose many plaintext messages to encrypt, establishing a feedback loop between choices. 

Relies on access to encryption function.

**Chosen-ciphertext**

The attacker chooses ciphertext to be decrypted.

Relies on access to a decrpytion function.

**Rubber-hose Cryptanalysis** 

Beat the cryptographic key out of them.

##### Security of Algorithms

The four categorizations of breaking algorithms are (in decreasing severity):

1. Total Break - find the key
2. Global Deduction - find a universal "decryption" algorithm that does not require the key
3. Local Deduction - deduce some plaintext from some ciphertext
4. Information Deduction - deduce some information about the key or plaintext.

Only the One Time Pad (XOR on a key of same length as the message) is a "perfect cipher" or "unconditionally secure," though it is not practical. 

Algorithms that are actually tenable are "computationally secure," that is, it requires a prohibitive amount of computing power in order to be broken. 

Attack complexity can be measured in the following ways:

1. Data Complexity - the amount of input data needed as input to the attack
2. Processing Complexity - The amount of time/work needed to perform the attack
3. Storage Requirements - The amount of memory needed to perform the attack

Many attacks are readily parallelizeable, though attacks only run in linear time. 

#### 1.2: Stenography

## Part I: Cryptographic Protocols

### Chapter 2: Protocol Building Blocks*

### Chapter 3: Basic Protocols*

### Chapter 4: Intermediate Protocols

### Chapter 5: Advanced Protocols

### Chapter 6: Esoteric Protocols

## Part II: Cryptographic Techniques

### Chapter 7: Key Length*

### Chapter 8: Key Management*

### Chapter 9: Algorithm Types and Modes*

### Chapter 10: Using Algorithms*

## Part III: Cryptographic Algorithms

### Chapter 11: Mathematical Background*

### Chapter 12: Data Encryption Standard (DES)

### Chapter 13: Other Block Ciphers

### Chapter 14: Still Other Block Ciphers

### Chapter 15: Combining Block Ciphers 

### Chapter 16: Pseudo-Random-Sequence Generators and Stream Ciphers

### Chapter 17: Other Stream Ciphers and Real Random-Sequence Generators

### Chapter 18: One-Way Hash Functions

### Chapter 19: Public-Key Algorithms*

### Chapter 20: Public-Key Digital Signature Algorithms

### Chapter 21: Identification Schemes

### Chapter 22: Key-Exchange Algorithms*

### Chapter 23: Special Algorithms fo Protocols

## Part IV: The Real World

### Chapter 24: Example Implementations

### Chapter 25: Politics

