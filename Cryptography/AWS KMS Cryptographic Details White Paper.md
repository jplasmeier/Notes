# AWS KMS Cryptographic Details White Paper

### HSA 

Hardened Security Appliance. Key generation occurs on dedicated HSAs without virtualization. 

### Digital Signatures

A digital signature is a scheme for demonstrating the authenticity of digital content. DS validates:

* Authentication (content created by a known sender)
* Non-repudiation (the sender cannot deny sending the message)
* Integrity (the message was not altered during transit)

The scheme usually consists of three algorithms:

* Key Generation (outputs a randomly generated public/private key pair)
* Signing Algorithm (input a message and a private key, output a signature)
* Signature Verifying Algorithm (input message, public key, signature and outputs true/false corresponding to the message's validity). 

More formally, a digital signature scheme is a triple of probabilistic polynomial time algorithms (G, S, V) such that:

* G (key generator) generates a public key `pk` and a private key `sk` on input `1^n` where `n` is the security parameter
* S (signing) returns a tag `t` on inputs `sk` and a string `x` 
* V (verifying) outputs a boolean accepted or rejected on the inputs `pk`, `x`, `t`.

For correctness, S and V must satisfy:

`Pr {(pk, sk) <- G(1^n), V(pk, x, S(sk,x)) = accepted} = 1`

A digital signature is said to be secure if for every non-uniform probabalistic polynomial time adversary A:

`Pr{(pk, sk) <- G(1^n), (x, t) <- A^S(sk, .)*(pk, 1^n), x not in Q, V(pk, x, t) = accepted} < negl(n) ` 

where `A^S(sk, .)` means that A has access to the oracle `S(sk, .)` and Q denotes the set of queries on S made by A, which knows `pk` and `n`, but cannot directly query `x` because that would be cheating. Also, `negl(n)` is the negligible function. 