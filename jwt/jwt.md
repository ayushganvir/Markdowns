# JWT - Json Web Token

[JWT reference](https://jwt.io/introduction)

JSON Web Tokens consist of three parts separated by dots (.), which are:

1. Header
2. Payload
3. Signature

Therefore, a JWT typically looks like the following.
```
xxxxx.yyyyy.zzzzz
```

## Header

The header typically consists of two parts: the type of the token, which is JWT, and the signing algorithm being used, such as HMAC SHA256 or RSA.

For example:
```

{
  "alg": "HS256",
  "typ": "JWT"
}
```


## Payload

Statements about an entity (typically, the user) and additional data.

```python 
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```


## Signature

To create the signature part you have to take the encoded header, the encoded payload, a secret, the algorithm specified in the header, and sign that.

```HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
  ```
  
One of the package to encode/decode JWT: [github python-jose](https://github.com/mpdavis/python-jose/)

