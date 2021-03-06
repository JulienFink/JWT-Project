# JWT-Project
Understanding, bypass and securing of Json Web Tokens (JWT)

* ## Prerequisites
Json Web Tokens allow the secure exchange of tokens between several parties. The security of the exchange translates into the verification of the integrity and authenticity of the data. It is carried out using multiple algorithms.

A token consists of three parts :
- A header, used to describe the token.
- A payload, which represents the information embedded in the token.
- A digital signature.

Example :
```
Header : {"alg": "HS256", "typ": "JWT"}
Payload : {"name":"JulienFink", "iat":1533777438}
Signature : 4V7KzBemrVji_kCyzGO3lQMZlBuVxryF3YhmMIr4kWI
```

The signature is crafted using a secret - 'Azerty123' in our case.

The "header", "payload" and "signature" parts are then encoded using base64url.

The complete JWT is a concatenation of these three parts, separated by a point (exception for the 'none' algorithm, which only has 2 parts - header and payload).

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoiSnVsaWVuRmluayIsImlhdCI6MTUzMzc3NzQzOH0.KJFzGjs_75Q56mY9QXqpEKU-wE2o3ufF3ZxfIUaexwI
```

Useful links for an overall understanding :
<br/> https://fr.wikipedia.org/wiki/JSON_Web_Token
<br/> https://jwt.io/
<br/> https://www.base64url.com/

* ## Abusing 'none' algorithm

Let us assume you don't have an account on a website, you don't want to create one and you decide to log in as a guest.
<br/> A JWT is assigned to your newly created session:

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJ1c2VybmFtZSI6Imd1ZXN0In0.
{"typ":"JWT","alg":"none"}.{"username":"guest"}.
```

What would happen if we decided to change the value of the username to 'admin' ? We would get a new valid JWT and thus get access to the admin section.

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJ1c2VybmFtZSI6ImFkbWluIn0.
{"typ":"JWT","alg":"none"}.{"username":"admin"}.
```

```diff
+ To avoid this type of Pr0bL3m, do not use 'none' algorithm.
```

* ## Signature stripping attack

What if we decided to change the algorithm to 'none' in the header part in order to exploit the previous vulnerability ? Let us try on a Root Me challenge (https://www.root-me.org/fr/Challenges/Web-Serveur/JSON-Web-Token-JWT-Introduction).

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0In0.OnuZnYMdetcg7AWGV6WURn8CFSfas6AQej4V9M13nsk
{"typ":"JWT","alg":"HS256"}.{"username":"guest"}.'a signature crafted using a secret unknown to us'
```

We can craft our own JWT using the 'none' algorithm :
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJ1c2VybmFtZSI6ImFkbWluIn0.
{"typ":"JWT","alg":"none"}.{"username":"admin"}.
```

Works !

![JWT blur](https://user-images.githubusercontent.com/64968597/135341742-d1aae0d8-deaa-4a66-9202-85529e982067.png)

```diff
+ To avoid this type of Pr0bL3m, use a whitelist of allowed algorithms. ['HS256']
- ['none']
```

* ## Brute forcing HS256/HS512 secret key

The HS256/HS512 algorithm uses a secret value to define the signature like so :
```
HMACSHA256/512(concatenation, 'my_very_secret_key')
```

If the secret key is weak enough, we can brute force it using several tools like 'jwtcrack'.
<br/> Let's take a closer look at this Json Web Token :
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJyb2xlIjoiZ3Vlc3QifQ.7cKjnSm7ilrnEGAiUEUaMMcr7I0_bfydc_ACt1Hk-9E
{"typ": "JWT", "alg": "HS256"}.{"role": "guest"}.'a signature crafted using a secret unknown to us'
```

If we try to forge a new JWT - i.e. replacing the value of role by 'admin' - without knowing the secret, we will get an invalid token.
<br/> Brute forcing the token becomes therefore an option to bypass the server filtering :

https://user-images.githubusercontent.com/64968597/135475547-054bada0-1296-42d6-bdfa-9c91487a8726.mp4

The secret value 'universe' is cracked under a second !
Since the same secret is used for each and every user, we can now craft our own 'admin' token with the newly discovered secret.

![jwtIO](https://user-images.githubusercontent.com/64968597/135476481-3a00ce9f-5ab4-4dae-bbff-b5a4ca28b0c1.JPG)

```diff
+ To avoid this type of Pr0bL3m, use strong shared secrets.
```

* ## Substitution attack for RS256 algorithm

-- Coming soon ! 
