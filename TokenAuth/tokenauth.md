TOKEN BASED AUTHENTICATION
Tokens store all the user information in an encrypted format and this token can be stored on the client side.
Here is the basic flow of token-based authentication

1.  The client sends a request with a username/password
2.  The application validates the credentials and generates a secure, signed token for the client
3.  the token is sent back to the client and stored there.
4.  when the client needs to access something new on the server, it sends the token through the HTTP server.
5.  the server decodes and verifies the attached token, if it is valid, the server sends a response to the client
6.  when the client logs out the token is destroyed.

![Screenshot from 2023-02-22 09-15-44.png](:/c2ceae63f6574314a0334664002e9fcf)

Benefits of token-based authentication

1.  Scalability and Statelessness - token authentication is truly stateless, since the server doesnt store client inofrmation, this way you can have a distributed system not relying on one server to recognise the sessionID
2.  More secure - since the token is encrypted is more secure than cookies, also tokens expire so after sometime it requires login
3.  Since they can be generated anywhere, it doesnt need to be validated by the server that generated it.
4.  tokens also help with authorization, with the token payload you can easily specify what resources the user can access. e.g if a 3rd party wants to access my gmail account that will allow that API to collect only my gmail info it will not be able to access other resources.

TYPES OF TOKENS

1.  Access Tokens - Are used to grant access to a protected resource, usually on first authenticating the client receives both an access and refresh token
2.  Refresh tokens - refresh tokens are used to obtain a new access token whne the current access token becomes invalid or expires. or even to obtain additional access tokens with an identical or narrower scope. it doent need to be credential information again.

Although the refresh token does not need the user’s credentials again to generate an access token, it still requires the client id and client secret (we will look at these terms in OAuth lessons). So even if attackers get a refresh token, they will not be able to get the access token.

go throufh airtel developer docs, for stk push
2\. confirm their api docs

JWT JSON Web Token
JWT is a standard that defines a safe, compact, and self-contained way of transmitting information between a client and a server in the form of a JSON object.
A JWT can either be signed (JWS) or encrypted (JWE) or both. If a JWT is neither signed nor encrypted, then it is called an insecure JWT.
JWT is one way of creating an access token. There are few alternatives to JWT such as Branca, Pasito, and Macaroon.
By signing the JWT, its integrity will be maintained. Other parties will be able to see the data in the JWT but will not be able to modify it.
By encrypting the JWT, its secrecy is maintained between two parties. Other parties will not be able to see the data, but if they change anything then we will not be able to find out.

Common use cases for using JWT

1.  Authorization - for example an app we are using wants some info from our email, we can authenticate ourselves on gmails auth server by giving it our credentials. gmail then gives us a JWT which our app can be used to get data from Gmail the token will contain information regarding what data can be accessed.
2.  Information exchange - JWT can be used to share information between two parties secretly.

JWT Structure
JSON Web token is basically three strings separated by a .(dot)
it looks like this

aaaaaaaaaaaaaaaa.bbbbbbbbbbbbbbbbb.cccccccccccccccc

![Screenshot from 2023-02-23 08-47-24.png](:/4bfe3493e8fa4110b3e2616bc6903428)

the first part - HEADER
Also known as the JOSE header. (JSON Object Signing and Encryption )This header describes what algorithm is used to sign or encrypt the data contained in the JWT.
The header defines two attributes.

1 alg: the algorith used to sign or encrypt the JWT
2\. typ: the content being signed or encrypted

![Screenshot from 2023-02-23 08-52-58.png](:/53b6ce943d4949b29d046134534c73a3)

now we encode it to base64encode, we get the first part of our JSON web token

NB This is just encoding and not encryption. Anyone can easily decode this string and get the JSON.

2.  PAYLOAD - the second part of the JWT
    it contains the main information that the server uses to identify the user and its permissions. the payload consists of claims, claims are statements about an entity(e.g the user and the additional data)
    a) Registered claim names
    these are reserved names that provide a starting point for a set of useful, interoperable claims

iss: identifies the principal that issued the JWT
sub: identifies the principal that is the subject of the JWT
aud: identifies that the JWT is intended for
exp: identifies the expiration time at the time or afte which the JWT MUST NOT be accepted for processing
nbf: identifies the time before which the JWT MUST NOT be accepted for processing
iat: identifies the time at which the JWT was issued.
jti: the JWT ID is a unique identifier for the JWT. The identifier value MUST be assigned in a manner that ensures that there is a negligible probability that the same value will be accidentally assigned to a different data object. this is useful in preventing the JWT from being replayed. its useful for one time use.

b) Public Claim Names - JSON Web Token Claims that can be defined at will by those using JWTs. ANy claim name SHOULD be either defined in the IANA registry, JSON Web Token Claims Registry, or be defined as a URI that contains a collision resistant namespace.

c) Private Claim Names - A producer and consumer of a JWT may agree to any Private claim name that is not a Reserved claim name or a Public claim name. Unlike Public claim names, these Private claim names are subject to collision and should be used with caution.

the payload looks like this

![Screenshot from 2023-02-23 09-24-23.png](:/91cabacc22e246e6b062be2354fdf656)

we then encode it using base64encode to get the 2nd part of our token.

![Screenshot from 2023-02-23 09-26-12.png](:/e9610ca9dd204c58bbbd6c08b86e5e44)

3.  Signature - the third part of the token
    it is created by combining the header and payload parts of the JWT and then hashing them using a secret key.

![Screenshot from 2023-02-23 09-38-29.png](:/dc5e1d1b9443465780dda44bbd840472)

this signature prevent anyone from trying to decode to get the payload

JWT VALIDATION
How the flow works

1.  The client sends a request to the server with their credentials
2.  The server generates a signed JWT for the client if the credentials are valid
3.  The server sends the token back to the client to be stored in the browser
4.  For every subsequent request the client sends the token back to the server.
5.  the server validates the token, if it is valid then it grants access to the requested resources.

How tokens are signed

Symmetric Encryption
When the JWT is signed using a secret key this is called a symmetric signature. This type of signature is done when there is only one server that signs and validates the token. the same secret key is used to generate and validate the token. The token is signed with HMAC.
HMAC(Hashing for Message Authentication Code) It's a mesage authentication code obtained by running a cryptographic hash function like (MD5,SHA1, SHA256) over the data being authenticated and a shared secret key.

Asymmetric Encryption

Asymmetric encryption is used where multople applications can validate a given JWT, because if you used symmetric encryption you would have to share the key with these different entities which isnt great for security.

asymmetric encryption uses a private-public key for signing. One server has a private key, the server generates the token, signs it using it's private key, and shares it with the client. the client can send this token to app and they can validate it with the private key.

![Screenshot from 2023-02-23 10-06-01.png](:/aec3dff4d6fa446a80336ce9a1ee6cdd)

How a token is validated

when the server receives a token, it fetches a header and payload from that token , it uses the secret key or the public key(asymmetric encryption) to generate the signature from ethe header and payload. if the generated signature matches the signature received it is then considered valid.

![Screenshot from 2023-02-23 10-15-49.png](:/6f4ac127d43542c48a4940ca28821faa)

Claim validations
because simply validating the signature of the token is not enough other things are needed

1.  Check if the token is still valid. This can be validated through exp claim.
2.  Validate that the token is actually meant for you through the aud claim.
3.  Check if the token can be used at this time using the nbf claim. NBF stands for not before which means that this token should not be used before a particular time.

Cryptographic Key Management

Secret Key Rotation - rotate your keys regularly so that even if a key is intercepted its use is limited by time.

Hacking JSON WEB TOKENS

1\. Brute Force Approach

In symmetric signing, we use a secret key to sign. If an attacker gets our secret key, the attacker can change the data in the token sign it again using the secret key and send it with the request. 

if the attacker has a valid JWT then they can brute force various symmetric keys and compare the signature to the known-valid signature. if there is a match then the attacker has discovered the symmetric key and can modify and forge JWTs at will. 

So choose a secret key carefully. 

NONE ALGORITHM

some libraries stilll allow keys with an algorithm set to None.

3) Modify the algorith RS256 to HS256