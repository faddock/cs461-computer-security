# Message Integrity
> Apr 8th 2025

By the end of this lecture you should know the following about MAC and digital signatures:
- Interface
- Security definition
- Common/recommended constructions
- Applications
- Relation to hashing and encryption
- How to achieve confidentiality + integrity

Cryptography (or Cryptology)
- Studies techniques for secure communication in the presence an adversary who has control over the communication channel

Message Integrity
- By integrity, we meant **tamper-evident**
- Symmetric vs. asymmetric 
MAC - Message Authentication Code (MAC) - Shared secret k -> MAC (symmetric)
Digital Signature 


Interface 

- MAC[k, m] -> t (called a tag)  
- Sender sends tuple (m, t)  
- receiver checks MAC(k,m0 == t?

message integrity will handle man in the middle attack

Digital Signatures
– KeyGen() → (vk, sk)
• A private signing key and a public verification key
– Sign(sk, m) → σ (called a signature)
– Verify(vk, m, σ) → True/False
- private key is for signing
- public key is for verification

UF-CMA vs. IND-CPA  
Similarities  
– Both give adversary access to many pairs of plaintext-ciphertext or message-MAC  
– Adversary can choose which message she wants to distinguish or forge  
- Key difference: adversary is not allowed to forge a message-MAC pair she has seen
– “Replay” attack is possible for MAC/signature

Unforgeability under Chosen Message Attack (UF-CMA)
• Interface: MAC(k, m) → t (called a tag)
• How do we define security of MAC?
– We pick a random key k
– Mallory can ask for MACs of any messages
– The attacker Mallory wins if she can produce a
forgery, i.e., (m, t) such that t = MAC(k, m)
• m must be a message Mallory did not ask MAC for

Sha-3 is pseudorandom - but sha2 is not
Merkle Damgard construction - absorbs chunks by chunks message - hash functions via this are not pseudorandom  
HMAC - hash-based MAC  
length extension attack may occur


## Digital Signatures
- asymmetric message authentication  
Interface  
– KeyGen() → (vk, sk)  
    - A private signing key and a public verification key  
– Sign(sk, m) → σ (called a signature)  
– Verify(vk, m, σ) → True/False  

HMAC(K, M) = H((K ⊕ opad) || H((K ⊕ ipad) || M))

K = secret key
•	M = message
•	H = hash function (like SHA-256)
•	ipad and opad = constants (inner/outer padding)

hash(secret || message)  
…which are vulnerable to length extension attacks, HMAC uses two rounds of hashing and masks the key with padding. This breaks the chaining structure that those attacks rely on.
``