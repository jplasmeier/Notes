# Udacity: Applied Cryptography

In chronological order rather than evergreen

## Lesson 1

### Symmetric Cryptosystems

plaintext:
m in M 

ciphertext c in C

encryption function
E: M -> C

decryption function
D: C -> M

for all m in M, D(E(m)) = m

### Principles

#### Kerckhoff's Principle

The method shouldn't matter, just some key that must not fall into the hands of the enemy.

So in these terms, the encryption/decryption functions can and ought to be public, and the keys are kept secret. 

#### Correctness Property

The decryption function applied to the encryption function applied to the plaintext message returns the plaintext message.

for all m in M, D(E(m)) = m

### One-Time Pad

#### Perfect Cipher

The ciphertext reveals no information about the key or the message.

More formally:

```
for m, m* in M
P[m = m* | E_k(m) = c] = P[m = m*]
```

The probability of `m = m*` given that `E_k(m) = c` is still just the probability of `m = m*`.

#### XOR Function

Exclusive OR is 1 when A or B is 1, but not both.

`A xor B xor A = B`

XOR is commutative and associative so:

`A xor A xor B = B`

#### OTP Definition

```
M = {0, 1}^n
K = {0, 1}^n

E_k(m) = c 
```

where

```
E_k -> k0k1k2...kn
m -> m0m1m2...mn
c ->c0c1c2...cn

c_i = m_i xor k_i
```

It's essentially just a bitwise xor with a private key. The key is a string of randomly generated bits.

#### OTP Perfect Cipher Proof

We need to show that the perfect cipher condition:

```
for m, m* in M
P[m = m* | E_k(m) = c] = P[m = m*]
```

holds for OTP.

Expanding the defition of Conditional Probability gives:

```
let:
A = m = m*
B = E_k(m) = c

P(A|B) = P(A n B) / P(B)
```

In this case:

```
P(B) = P(E_k(m) = c) = 1/|K|
P(A) = 1/|M|
P(A n B) = P(m = m*)/|K|
P(A|B) = P(m = m*)/|K| / (1 / |K|) = P(m = m*)
```

So the probability of knowing if messages are equal does not change if you know the ciphertext and encryption function. 

#### OTP Analysis

OTP is a Perfect Cipher, but not very practical. For a message of `n` bits, the key is exactly `n` bits long. 

It's also malleable which means that the message can be changed without triggering decryption failure.

#### Shannon's Theorem

If a cipher is perfect, then |K| > |M|.

##### Proof:

Assume E is a perfect cipher where `|M| > |K|`. Let `c_0 in C` with `P(E_k(m)) = c_0 > 0`. 

Decrypt c_0 with all k in K. `D_k(E_k(m)) = m`. The set of messages `M_0 = Union(k in K, D_k(c_0))`. Then there exists some `m* in M` where `m* not in M_0`. Then, `P(m = m*) = 0`, which is contradiction to `P(m = m*|E_k(m) = c) = P(m = m*)`. 

### Lorenz Cipher

A machine generates a key sequence based on its configuration. The key is XOR'd with the message and sent as ciphertext. 

Configuration was synchronized in secret (symmetric encryption). 

If you intercept two similar ciphertext messages, you can XOR them together and get information about the key. 

## Lesson 2

Applications of symmetric ciphers.

### Randomness

What's random? Tough question.

What isn't random? A repeating pattern isn't. A sequence with a bounded number of repeating elements isn't. 

“Anyone attempting to produce random numbers by purely arithmetic means is, of course, in a state of sin." - von Neumann

#### Kolmogorov Complexity

The Kolmogorov complexity of a sequence S is the length of the shortest possible description of S. For a sequence to be random, we say that `s` is random if `K(s) = |s| + C`. That is, if the Kolmogorov complexity is at least as high as the length of the sequence, plus a constant term. 

For example, if a short program can compute the entire sequence, it cannot be random. 

However, Kolmogorov Complexity is not computable.

#### Statistical Tests

One can show how non-random a sequence is using statistics.

#### Generation Method

Ultimately, the best way to satisfy randomness is to show that the following property holds:

For a sequence `s = x0x1x2..., xi in [0, 2^n-1]`, if an attacker knows `x0x1...xm-1`, they must only be able to guess the value of `xm` with probability `P = 1/2^n`

#### Physically Random Events

* Quantum Mechanics
* Thermal Noise
* User actions (key presses, mouse movements)

### PRNGs

Pseudo Random Number Generators take a randomly generated seed and produce a longer sequence of (mostly random) bits based on the seed.

### Modes of Operation

#### Electronic Codebook Mode

One to one mapping between input and output. Does not hide repetition. Vulnerable to scrambling. 

#### Cipher Block Chaining (CBC)

Each message block is encrypted with the key XOR'd with the output of the previous block. For the first block, an initialization vector is XOR'd with the first message block.

#### Counter Mode (CTR)

For each block, first encrypt the value of a counter appended to a nonce (one-time use random value). For a 128 bit cipher one could use a 64 bit nonce and a 64 bit counter, concatenated. Then encrypt and XOR the output with the message block.  

Note that CTR can be parallelized and CBC mode cannot because it is dependent on the previous block. One could even precompute the outputs of the encrypted blocks before the message is known using CTR mode. 

#### Cipher Feedback Mode (CFB)

CFB is sort of a hybrid between CTR and CBC. Part of the previous input to the encryption function is concatenated with part of the resulting ciphertext block. The message is XOR'd with the portion of the output that will be sent to the next block's encryption function. 

### Protocols

A precisely defined sequence of steps (computation or communication) or procedure with two or more participants. 

Cryptographic protocols involve keeping secrets secure. 

### Padding

If you have a 128 bit block cipher and want to encrypt fewer than 128 bits, you need to account for this somehow. We do this by padding the input with extra 0s. 

### Cryptographic Hash Functions

A function that maps from a (infinitely) large domain to a small fixed output, ideally well-distributed across the domain.

Additionally, we need the following properties:

* pre-image resistance
	* Given h = H(x), it's hard to find x.
* weak collision resistance
	* Given h = H(x), it's hard to find any x' such that H(x') = h.
* strong collision resistance
	* Hard to find any pair x and y such that H(x) = H(y).

### Random Oracle

An ideal cryptographic hash function such that H(x) -> h gives a uniform distribution.

However, it is not possible because it is deterministic and cannot add randomness to the output.

In practice, we can get close. 

#### Probability of Guessing

P(one guess H(x') = H) = 2^(-b)
P(guess is bad) = 1 - 2^-b

where b is the number of output bits

Strong collision resistance requires at least twice as many output bits of the hash function. A weak collision would need ~N guesses, a strong collision would need N^(-1/2) guesses. 

SHA-1 used 160 bits, but one can find a collision with 2^31 guesses.

SHA-2 uses 256 or 512 bits but this can be compromised. 

### Storing Passwords

Obviously don't store passwords in plaintext. 

But don't just encrypt the passwords either. If the key is compromised, all passwords are known. Also, you can determine the length of the password, and whether or not two users have the same password. 

#### Early Unix Implementation

Encrypt 0 with the password as the key several (25) times. 

However, DES used only 52 bit keys and they were always ASCII characters. Additionally, dictionary attacks make it easy to pre-compute a list of 25-times encrypted 0 ciphertext.

### Salted Password Scheme

Each user gets n=12 random bits. The password is stored as the hash of the salt concatenated with the password. Even if the passwords are the same, the results will be different due to the salt. 

Adding salt makes it not much more difficult to break the password if the password file has been compromised. But, it does make dictionary attacks much less effective. 

### Password Reuse

Password reuse exposes attacks from shoulder surfing, keyloggers, and other means of picking up someone's password. 

### Hash Chain

Compute the hash of a hash of a ... of a secret. The server stores the result of the last value n. The user will login with the n-1th hashed secret. The next time, the user will send in that n-1th value. The server hashes that value again and checks that they are equal. 

This is also known as the S/Key system. However, it is not terribly practical. 

## Lesson 3

