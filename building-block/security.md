# Security

1. Symmetric (SHA256)
2. Asymetric (Ed25519)

## Symmetric
* Alice and Bob shared the same secret key


### Benefits
* **Fast**: HMAC-SHA256 is fast and often hardware accelerated, and much faster than any asymmetric scheme.
* **Simple**: Symmetric signatures are much more simple and quick to get started with than asymmetric ones.
* **Ubiquitous**: HMAC-SHA256 is widely available on every platform and language.
* **Warning**: Treat the signing key as any other cryptographic secret. If you do not control the security of both the producer and consumer it is recommended you use an asymmetric signature instead.

### Challenges


## Asymetric
* It's a standard method for secure communication over the internet, including HTTPS, email encryption (PGP), and more.

### Encryption and decryption
* Encrypt using public key by the client
* Owner can decrypt with the private key (only private key can decrypt)

### Digital signature
* Server creates a digital signature by encrypting the payload using private key
* Client recieves the payload and signature and need to verify the author and content
* Client encrypts using the public key and compare


### Benefits
* **Security**: The private key does not need to be transmitted or revealed to anyone.
* **Non-repudiation**: Due to digital signatures, the sender cannot deny having sent a message.

### Challenges
* **Performance**: Asymmetric signatures can be more CPU intensive to produce and verify than symmetric ones.
* **Complex**: Key management is complex
