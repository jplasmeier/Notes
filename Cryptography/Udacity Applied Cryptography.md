# Udacity: Applied Cryptography

In chronological order rather than evergreen

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