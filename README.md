# Projet-JWT
Understanding, bypass and securing of Json Web Tokens (JWT)

* ### Prerequisites
Json Web Tokens allow the secure exchange of tokens between several parties. The security of the exchange translates into the verification of the integrity and authenticity of the data. It is carried out using multiple algorithms.

A token consists of three parts:
- A header, used to describe the token.
- A payload, which represents the information embedded in the token.
- A digital signature.

Example:
<br/> Header: {"alg": "HS256", "typ": "JWT"}
<br/> Payload: {"name":"JulienFink", "iat":1533777438}
<br/> Signature: 4V7KzBemrVji_kCyzGO3lQMZlBuVxryF3YhmMIr4kWI (crafted with a secret - "Azerty123" in our case)

The "header", "payload" and "signature" parts are then encoded using base64url.

Complete JWT: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiSnVsaWVuRmluayIsImlhdCI6MTUzMzc3NzQzOH0.KJFzGjs_75Q56mY9QXqpEKU-wE2o3ufF3ZxfIUaexwI

Useful links:
<br/> https://fr.wikipedia.org/wiki/JSON_Web_Token
<br/> https://jwt.io/
<br/> https://www.base64url.com/

* ### Absusing 'none' algorithm

* ### Signature stripping attack

* ### Brute forcing HS256 secret key

* ### Substitution attack for RS256 algorithm
