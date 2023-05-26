Because http is stateless, you want to avoud repeatedly sharingg login info each time you send a request.
Session authentication was invented to get around this problem.

Steps to create a session between a user and a web server.



![Screenshot from 2023-02-22 08-24-02.png](:/d04f524fd9294731a229748a4b934223)



1. The user sends a request to the server, the request contains login credentials of the user and the info it is requesting.
2. The web server authenticates the user, it creates a session and stores all the information about the user in memory or a databse and returns a sessionID to the user.
3. the session id is stored by te user in browser cookies and is then used to make future requests. It is sent in the HTTP header.
4. the web server checks if the sessionID and checks if it has any info regarding this sessionID 
5. if the sessionID is valid then the web server recognises the user and returns the requested information



![Screenshot from 2023-02-22 08-29-38.png](:/23ee4307e203456eae9aefb3d4dd6ca3)


Limitations of Session Based Authentication
1. in distributed systems the request doesnt always go to the same server. you could authenticate on server 1 and then update cart is received by server 2
2. storing and retrieving the session information from the database or memory is a costly process, checking the sessionID in the db or memory is costly as well.
3. cookie fraud - it is possible that a malicious user or a website can access your cookies then carry out malicious operations(CSRF attack)


