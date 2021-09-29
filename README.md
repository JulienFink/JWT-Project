# Projet-JWT
Understanding, bypass and securing of Json Web Tokens (JWT)

* ### Prerequisites
Json Web Tokens allow the secure exchange of tokens between several parties. The security of the exchange translates into the verification of the integrity and authenticity of the data. It is carried out using multiple algorithms.

A token consists of three parts:
- A header, used to describe the token.
- A payload, which represents the information embedded in the token.
- A digital signature.

§Example:§
Header: {"typ": "jwt", "alg": "HS512"}
Payload: {"name":"JulienFink", "iat":1533777438}
Signature: {cAOIAifu3fykvhkHpbuhbvtH807-Z2rI1FS3vX1XMjE}

The "header" and "payload" parts are encoded using base64url.

Complete JWT: eyJ0eXAiOiAiand0IiwgImFsZyI6ICJIUzUxMiJ9.eyJuYW1lIjoiSnVsaWVuRmluayIsICJpYXQiOjE1MzM3Nzc0Mzh9.cAOIAifu3fykvhkHpbuhbvtH807-Z2rI1FS3vX1XMjE

Here is a typical JWT:

* ### Absusing 'none' algorithm

* ### Signature stripping attack

* ### Brute forcing HS256 secret key

* ### Substitution attack for RS256 algorithm
