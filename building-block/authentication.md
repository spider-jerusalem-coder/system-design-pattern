# Authentication

## JWT
* JSON Web Tokens (JWT) are a popular method for securely transmitting information between parties as a JSON object.
* Communicate claims between clients.

### Benefits
* **Stateless**: Enable stateless authentication. (prevent server side storage)
* **Authenticity of the sender**: It can be signed using symmetric or assymetric key to validate the authenticity of the sender.
* **Language agnostic**: Its a standard that is independent of the language


### Challenges
* **Token Revocation**: Token is not stored at server so not immediate revocation
* **Cross-Site-Scripting-Attacks**: As token is stored in local storage, its prone to the cross-site attack. Javascript running on the user brwser can read the token stored in local storage or session storage

### Overcome
* **Server side blacklisting**
* **XSS Attack**: Implement Content Security Policy (CSP) TODO 
