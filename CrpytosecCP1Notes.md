# 3.1.6 

A weak hashing algorithm

```python
from Crypto.Cipher import AES
ciphertext = load_ciphertext_we_gave_you_as_bytes()
key = load_key_we_gave_you_as_bytes()
iv = load_iv_we_gave_you_as_bytes()
cipher = AES.new(key, AES.MODE_CBC, iv=iv)
plaintext = cipher.decrypt(ciphertext)
# ciphertext must be multiple of 16 bytes
```

# 3.1.5
public key - is given to everybody for encryption  
private key - secret key used for decryption  
RSA decryption  
- e - public prime  
- n - public modulus  
- d - secret  
- m - plaintext  
- c - cipher-text  

encryption: $c = m^e mod(n)$  
decryption: $m = c^d mod(n)$  
checkpoint 2 goes over why only specific numbers will work
this is called modular exponentiation

pow(base, exp, modulus) -> does base**exp mod(modulus)
use this instead of base**exp % modulus (WHY?)



