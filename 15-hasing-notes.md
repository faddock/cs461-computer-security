Cryptography studies techniques for secure communication in the presence of an adversary who has control over the communication channel  
    Alice and Bob want to send messages to each other (eg over the internet) - want confidentiality and integrity
    Eve is an eavesdropper 
    Mallory is malicious
cryptography also studies techniques for secure storage, and secure collaborative computation - 
CS 461 will focus on communication and on breadth, instead of depth
Goals - 
    crpyographic hashing
    symmetric and asymmetric encryption 
    message auth codes & digital signatures
        
Know the interfaces of basic crypto primitives
    – What are their inputs and outputs
    – What it means for them to be secure
    – What guarantees they provide and not provide
    – Where and how they are typically used
    – Which schemes to use when you need one

modern cryptography is heavily based on mathematics - but has to resort to assumptions

empirical - concerned with verifiable by observation rather than theory or logic


Cryptographic hash Function takes airbitrary length input, given fixed length output
same input always produces same output (MD5, SHA1, SHA2, SHA3)
ex: Sha3-256("welcome") = some output
ex: Sha3-256("Welcome") = different output

Is there a connection between hashmap hash function, and cryptographic hash function?
There are significant differences btw non crpytographic and cryptographic hash functions

    Ø Applications
        ○ Password hashing
            system stores username, user submits, system comuptes
            (uses one way property)
        ○ integrity of external storage
            store hash of file locally, then compare hash upon download to check if mallory was involved
            (uses collision resistance 
    Ø Desired properties of crpyto hash function
        ○ hard to invert (one-way OW)
        ○ hard to find collisions (collision-resistant, CR)

more formal definition of cryptographic hash function H with n-bit output is a function:
y = H(x):{0, 1}* -> {0, 1}^n
this is simply a notation that a binary notation of arbitrary length *, dmaps ot a binary string of length n

one-way (aka preimage resistance) - given any y, it is infeasible to find x s.t. H(x0 = y
collision resistance (CR) - infeasible to find x and x' s.t. x != x' and H(x) = H(x')

infeasible != impossible 
Cryptographic attacks, such as inverting a hash or finding collisions, are theoretically possible but computationally impractical.
Inversion: Given a hash h = H(x), finding x (pre-image attack) is possible via brute-force search, but it requires checking 2^n values, making it impractical for large n.
Collisions: Since hash functions have a fixed output size but an infinite input space, collisions must exist (by the pigeonhole principle).

Brute-force inversion occurs in O(2**n) time - 

    Explanation - we expect hash function to behave like a random function - 

brute-force collision in O(2**(n/2)) time and space due to birthday paradox - a probability puzzle that demonstrates how surprisingly quickly the probability of two people in a group sharing a birthday increases as the group size grows


Pigeonhole principle, also known as Dirichlet's principle, states that if you have more items than containers, at least one container must contain more than one item.
NOTE - DO NOT ROLL YOUR OWN CRYPTO

formal hash function definition
A crpyotgraphic hash function H with n-bit out put is a function: $y = H(x):{0,1}*  {0, 1}^n$
- one-way (OW aka preimage resistance): for almost all y, infeasible to find x such that H(x) = y

### Proof by Contradiction: One-Wayness of Merkle-Damgård

#### Theorem:
If $ f $ is a one-way function, then the Merkle-Damgård construction using $ f $ is also one-way.

#### Proof:

1. **Assume the contrary**: Suppose the Merkle-Damgård hash function $ H(x) $ is not one-way.
   - That means there exists an efficient algorithm that, given $ y = H(x) $, can find a preimage $ x $ such that $ H(x) = y $.

2. **Structure of Merkle-Damgård**:
   - The input $ x $ (after padding) is split into blocks $ b_0, b_1, \dots, b_{m-1} $.
   - The function iteratively applies $ f $, where each step depends on the previous step’s output.

3. **Finding a Preimage for $ f $**:
   - The last step of the hash function is $ f(u \| b_{m-1}) = y $ for some intermediate value $u$.
   - Since we assumed we can efficiently invert $ H(x) $, we can find $ x $, and thus recover $ u $.
   - This means we have efficiently found a preimage for $ f $, contradicting the assumption that $ f $ is one-way.

4. **Conclusion**:
   - The assumption that Merkle-Damgård is not one-way leads to the contradiction that $ f $ is also not one-way.
   - Therefore, Merkle-Damgård must be one-way if $ f $ is one-way.
   - $\boxed{\text{QED}}$









 
    
        


