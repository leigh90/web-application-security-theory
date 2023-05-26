Securing websites, mobile apps and web APIs
# <font color="orange">Cross-site Scripting Attacks(XSS)</font>
These are attacks in which a malicious script is added to a website. when a user accesses this website then accidentally run this malicious script, compromising their data as the attacker gets control of the use'r browser.

## <font color="green"> Types of Cross-site Scripting Attack</font>
### <font color="aqua">1. Stored Cross-site Scripting Attack</font>

This is typically targeted at websites that allow user input and storing it in the database. 
For example in a website whose input allows html tags to be embedded in the sites comments section. The embedded tags become a permanent feature of the page, causing the browser to parse them with the rest of the source every time the page is opened.
from this point on every time the page is accessed the html tag in the comment will activate the script that was embedded. likely that has the ability to steal visitor's session cookies which the attacker can use to access the user's personal information.
Malicious scripts can attack the user through the following methods
- installing browser-based keyloggers to capture keystroked of the victim. this can be dangerous as the attacker might use to get access to social media passwords, email passwords, cerdit card info etc.
-  capturing session cookies that can be used to trigger CSRF
-  redirecting users to malicious websites


![5315239600979968.png](:/19eb6969e01b4c7dbe919edf3dd0c9b3)

### <font color="aqua">2. Reflected Cross-site Scripting Attack</font>

the attacker tricks the user into clicking a link that contains the malicious script. the user receives the malicious link through email, search results, ads or another website.

as soon as the user clicks on the links the malicious script is executed on the user's browser. this script can steal browser data such as cookies and send them to the attacker.


![4653816516444160.png](:/76fd80c4cead46adbc43d1d442882ab0)

How to prevent XSS

1. Ensure all your inputs are validated before they are stored into the db. The idea is to trea all inputs as malicious till they pass certaiin length and type requirements.
2. sanitizing user input - this involves cleaning all user data of potentially dangerous symbols that are usually used in HTML markup and JS code. you can use tools that blacklist HTML tags in user input.


# <font color="orange">Cross-site Request Forgery (CSRF)</font>
CSRF is an attack that tricks a web browser into executing an unwanted action in an application after a user logs in. It allows an attacker to force a logged in user to act without their consent or knowledge. say force a user to transfer funds from a banking website or share important info.


How does CSRF work?
CSRF relies on 2 things cookie-based session handling and predictable request parameters. 
Session handling is useful coz the user is already logged in and the website relies on cookies to identify the user,
predictable  The requests that perform the malicious action do not contain any parameters whose values the attacker cannot determine or guess.

Now, suppose a request to transfer funds looks like this:
Suppose a user, Alex, is a customer of ABC bank. He is logged into the bank website. This means the session is currently active, and login information is maintained in the cookies.

`GET http://abcbank.com/transfer.do?acct=Bob&amount=＄500 HTTP/1.1`
The attacker will create a request in which the account details of the attacker will be provided.
`GET http://abcbank.com/transfer.do?acct=attacker&amount=＄500 HTTP/1.1`
The attacker has created the request. The only thing they need to do now is trick the user into sending this request from their own browser.
The attacker can create a promotional email which it will send to the user. This email will contain a hyperlink as shown below:
`<a href="http://abcbank.com/transfer.do?acct=attackerA&amount=＄500">Get the offer!</a>`
If the user clicks on the hyperlink then the transaction will go through and money will be transferred to the attacker’s account.
the user must already be logged in for this attack to be successful. Otherwise, the user will get a login prompt and become skeptical of the link.


<font color="green">CSRF attack using POST request</font>
in the case of a POST requesy tthe user's browser sends parameters and values in the request body and not the URL in the case of a GET request.

Therefore, tricking the user to operate a POST request is a bit difficult. With a GET request, the attacker only needs the victim to send a URL with all the necessary information. In the case of POST, a request body must be appended to the request.

The attacker can create a website and add a JavaScript code to it. They will then send the link of this website to the user through a phishing email.
When the user clicks on the email, the malicious website will send a POST request to the bank application.

The code of malicious website created by the user will be as shown below:
```
<body onload="document.csrf.submit()">
 
<form action="http://abcbank.com/transfer" method="POST" name="csrf">
    <input type="hidden" name="amount" value="500">
    <input type="hidden" name="account" value="attacker">
</form>
```
<font color="green">How to prevent CSRF attackes</font>
1. CSRF TOKENS
Also known as anti-CSRF token or synchronizer token. an anti-csrf token is a type of server-side csrf protection
this is basically a string that is only known to the user's browser and the web application. 

when a web server receives a request it generates a token and stores it, the token is statically hidden set as a findden firld of a form.
when the form is submitted by the user the token is included in the post request data
the application compared the token generated and stored by the applocation with the token sent in the request, if they match the request is valid otherwise it's considered invalid and is rejected.
even if an attecker created a malicious request, it is not possible to add the token as the attacker wouldnt be aware of it.

2. SAME SITE COOKIES
this is used to deal with attacks where attackers create a different website that sends a s POST request that is activated when a user clicks on it through a phising attack.
if a session cookie is marked as a samesite cookie it is only sent along with requests that originate from the same domain. therefore even if the user clicks on the hyperlink provided by the attacker the cookie wont be sent 

best practices to avoid a csrf attack

always log out of the website once you're done, this closes the session and removes cookies
try not to use multiple websites at the same time. if you're loggged into a website in one browser tab ans using a malicious website in another tab, then chances of a csrf attack increase
don't allow browsers to remember passwords


<font color="green">DENIAL OF SERVICE</font>

here an attacker tries to crash an application so the legitimate users are not ablet to access the application. Typically the attacker doesnt benefit from this attack, the goal is to harm the organisation.
<font color="green">TYPES OF DENIAL OF SERVICE ATTACKS</font>
1. FLOOD ATTACK
The attacker floods the application with more requests than it can handle per second forcing the server to slow down or eventually crash.

a) ICMP FLOOD - the attacker leverages misconfigured network devices by sending spoofed packets that ping every computer on the network, instead of just one machine, the network then applifies the traffic. (also known as "smurf attack" or "ping of death")
b) ASYN FLOOD - the targeted server received a request to begin the handshake but the handshake is never completed, leaving the connected port occupied and unable to process further requests, the attacker continues to send more and more requests, overwhelming all open ports and shutting down the server.

2. CRASH ATTACK
The attacker transmits a bug to the server which then takes advantage of the vulnerabilities of the server.

DIstributed denial of Service
multiple systems work orchestrate a synchronized DOS attack on a single target.
the target is attacked from many locations, at once instead of being attacked from a single location.

the distributed DOS attack is hard to stop because:
the attack is didtributed across mutliple locations,it's difficult to counter-attack multiple machines.

Steps to stop DoS attack
1. Black Hole Routing
Once you realise you're being attacked you send all traffic(legitimate and illegitimate )to a black hole.
so the application goes down regardless. but it gives you time to figure out what the origin of the attack is and take appropriate action.

2. Rate Limiting
limit the number of requests a server will accept in a certain amount of time. you will still affect some valid requests but alteast you wont overwhelm the system.




