
Cryptography → is a science for hide info by change its format from plain.txt to cipher.txt :

# Hashing 
- Hashing → is a mathematical operations that execute on the user input (data) and return output always in fixed size
    - Irreversible (one way hashing)
    - fixed size
    - like : md4 , md5 , sha1 , sha2 ,sha256, ….
    - most web sites use rainbow tables (you)
    - its range from 128bit to 512bit
    - sha1sum or sha256sum or ….. → used for hashing files
---
# Encoding

- Encoding → changing in the format of the data so that can different systems manipulate with, not for security.
    - first one was “rot13” → move it 13 places in the alphabetic (was joke in the beginning)
    - examples : rot13 , base64 , Hex , URL
    - you can encode the JS injection like alert, if the WAF blocked it.
    - it is not one way you can , you can decode it
    - converting between number systems and ascii code considered encoding
    - ex:
        - endian representation
            - big endian → the sequence hex representation of its characters.
            - little endian → reveres of the sequence hex representation of its characters.
---
# Encryption

- Encryption
- Types :
    - symmetric encryption → using one key to encrypt and decrypt the data
        - Ceaser cipher considered the first encryption method (Although it is like encoding)
        - Examples: AES (famous) , DES (old but still used), 3DES , TwoFish , BlowFish
    - Asymmetric encryption → use two keys public one for encryption and private one for decryption (the receiver has two keys public and private ,and these passwords interconnected, so he can share with you the public key, so you can encrypt the message with it, and only the receiver who has the private key can decrypt it)
        - you can using ssh with public and private keys rather than normal login
            - `ssh-kegen` → generate public and private (mostly be 2048 character) keys
                - `-t <type>` → type of encryption like rsa
                - here you send the your public key to the server, authentication process (the server creates a symmetric key and encrypt it with your public key, then it resend it to you, so if you the guy who sand the request you can decrypt the symmetric key with private key which you have)
        - Examples: RSA , DSA , DH , ECDH
- it’s common for base64 encoding to end with ==