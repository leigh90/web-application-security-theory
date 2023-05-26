## <font color="green">ENCRYPTION</font>
WHAT IS ENCRYPTION?
Encryption is used to protect data in transit

Encryption is basically converting plain text into cyphertext(text that is not understandable by the third party). 
It requires the use of an encryption key and encryption algorithm, to encrypt/decrypt the plain text into ciphertext.
How the key is used is defined by the encryption algorithm.
both the sender and receiver must have the encryption key.



![encryption.png](:/db993741cb9640019c0b3d990c048e37)


### <font color="green">Types of Encryption</font>

1, SYMMETRIC ENCRYPTION
The same key is used for both encryption and decryption.

Its like locking a message in a box then using duplicate keys to open the padlock.
The main disadvantage is that you need to send  the key to each other, the attacker can always intercept this key in transit and be able to read the messages.
the keys used in symmetric encryption are not large usually a max of 256bits

a big advantage is that it is fast, and is used to encrypt large amounts of data.

2. ASYMMETRIC ENCRYPTION
The sender and receiver use a separate key to decrypt the message (PKI(Public Key Infrastructure))
the advantage of this kind of encryption is that keys are not  shared over the network so it cant be intercepted
it uses a public-private key model

the recipient sends a public key to all the senders, the senders encrypt the message using this public key. the receiver used the their private key to decrypt the message. the keys used in asymettric encryption are fairly large ans can be upto 2048 bits

the drawback with this is that it is slow. It is normally used to exchange the encryption key safely, after that symmetric encryption is used for communication 

What is an encryption algorithm?

this is a math formula used to transform data into ciphertext. an encryption algorithm uses an encryption key to transform plaintext into cipher text. The ciphertext can then be decrypted to plaintext using a decryption algorithm.

some commin algorithms

aes
des
Blowfish
Twofish
RC4, RC5, RC6

What is a brute force attack?

In a brute force attack, 

this is when an attacker tries to guess the decryption key.
typically the attacker uses a program to do this so its important to use a strong key that makes it impossible or not worth the effort to try and compute all the possible combinations


Let’s see what we mean by a strong key. We know that data is represented by bits in computing language. Each bit can have a value of 0 or 1. If a key is 2 bits long then there are four possible combinations, i.e. 00, 01, 10, 11. This is very easy for computers to crack.

Let’s take a key which is 256 bits long. The total number of possible combinations are 2^{256}
2 
256
 
. A 256-bit private key will have 115, 792, 089, 237, 316, 195, 423, 570, 985, 008, 687, 907, 853, 269, 984, 665, 640, 564, 039, 457, 584, 007, 913, 129, 639, 936 possible combinations. No supercomputer can crack that in any reasonable timeframe. So, to prevent brute force attacks, the key should be of sufficient length.

## <font color="green">Understanding SSL certificates</font>

What is an SSL Certificate?

SSL(Secure Sockets Layer) certificates create an encrypted environment between a client and server, an SSL certificate is a small data file installed on a web server that allows for a secure connection between the server and web browser.

the certificate is  a base64 encoded and contains the following info:
	- name of the entity to which the certificate was issued
	- the public key required for encryption and digital signature verification
	- the digital signature created with the private key of the certificate issuer
	
	SSL is a protocol that is used to secure the HTTP. SSL is deprecated now and Transport Layer Security (TLS) protocol is used instead. Most SSL certificates today also support the Transport Layer Security (TLS) protocol, which is considered to be more secure than SSL.
	
	the application owner should install the SSL certificate on its web server, when an application is secured by an SSL certificate then its url starts with https instead of http
	that little lock symbol in the url tells you the website is secured by a certificate which you can click on to get details of the certificate like which Certification Authority issued this certificate.
	
	A Certification Authority (CA) is a company or an organization which is trusted to sign, issue and revoke digital certificates. Some of the most popular certification authorities are Sectigo SSL, Symantec SSL, RapidSSL, GeoTrust SSL, and Thawte SSL.
	
### <font color="green"> SSL certificates validation level</font>

before a CA issues an SSL certificate it has to validate the requesting organisation. it needs to ensure the org is doing legal buisness and owns the domain.
there are 3 types of validations

1. Domain Validation Certificates

suitable for websites that just need encryption and nothing more 
all you need to do is prove control of your domain, the issued certificate contains domain name that was supplied to the certification authority within the certificate request

2. organisation validation certificates
applicant needs to prove that their companu is a registered and legally accountable business.
the ov ssl privided a way for customers to check the cerified business information in the certificate details section

3. extended validation certificate 
requires alot of validation before its issued
this is suitable for applications that ask for confidentials of users like credit card numbers.

	
Types of SSL certificates
Single-name ssl certificates
a single-name SSL certificate can nly be used on a single domain or IP. its not applicable on sub-domains. 

Wildcard SSL certificate
a wildcard SSL certificate is applicable to a domain and all its subdomains. this certificate will be applicable on all its sub-domains will also have the certificate

multi domain SSL certificate
multi-domain SSL certificate lists multiple domains on one certificate, with an MDC, domains that are not subdomains of each other can share a certificate, For example, domains like www.mysite.com, www.example.com, and www.abc.com can share the same certificate.

