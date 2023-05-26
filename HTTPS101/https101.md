From the beginning HTTP has been used to transfer info between the client and server. 
the data is transferred in plain text format throught http, meaning an attacker can acees to the data that is transferred over the internet. 
this wasnt very secure.

Eventually HTTP was enhanced with some encryption using the SSL encryption protocol being added on top if it.
this combination is called HTTPS(HTTP Secure) its basically (http + ssl)
in 1999 TLS was introduced which was more efficient than SSL


Benefits of using HTTPS over HTTP
1. Authenticity - ensures the client is talking to the intended website since it provides its identity to the client.
2. Confidentiality - https encrypts the data being transferred between client and server keeping the identities of either secret
3. Message Integrity - https ensures the data is not modified in transit, since it gives the client and server as a way to verify that the data has not been tampered with.

How does HTTPS work?
1. First the client first verifies that the server it is talking ti us the correct server and how the data will be encrypted - this is called the TLS HANDSHAKE
TLS Handshakes are a series of datagrams or messages exchanged btwn client and server.
STEP 1 - The client initiates the handshake by sending a 'hello' message to the server which includes the 
- the TLS version the client supports
- the cipher suites it supports
- a string of random bytes called *client random*

Step 2 - if the capabilities of the client and server match the server replies to the client with the following information:
- the cipher suite it has selected for the communication
- an SSL certificate
- a randomly selected prime number called *server random*

STEP 3 - Certificate Validation
the client verifies that the SSL certificate provided by the server is valid by checking some properties if the certificate
- the certificate's digital signature
- the cert isnt expired or revoked
- it contains the correct domain name

STEP 4 - PREMASTER SECRET 
The client generates a random string of bytes called a pre-master secret which it encrypts with the server's public key which it gets from the ssl certificate and then transmits it ti the server. this is key because Let’s suppose an attacker is listening to all the messages while a connection is being established. The attacker can get client random and server random, but it cannot get the pre-master secret. This is because the pre-master secret is encrypted using the server’s public key. Only the server can decrypt it, using its private key. So, the attacker will not be able to calculate the session key.

STEP 5 - SESSION KEY CREATION
Now both the client and the server calculate the session key using the client random, server random and the pre-master secret. they will obtain the same session key.

STEP 6 - CLIENT FINISHED 
The client sends a finished message encrypted by the session key

STEP 7 - SERVER FINISHED 
The server sends the finished message encrypted by the session key

STEP 8 - SYMMETRIC ENCRYPTION SUCCESSFUL
The client and the server will encrypt their messages with the same session key.
