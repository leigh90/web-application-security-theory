OPENID Connect

OAuth 2.0 is designed only for authorization. while it can be used for authentication, it's not ideal, but it was extended to support authentication.

OpenID Connect is an extension of OAuth. It is a thin layer above OAuth which adds support for authentication.
When an app prompts you to authenticate using google or facebook, the app is probably using OpenID connect which allows clients to receive information about authenticated sessions and end-users.

OPENID Connect
The challenge of local authentication is maintaining a db for each application can be tedious and also sign up and confirmation can be tedious users end up using the same password which is bad for security.

IDENTITY PROVIDERS
Identity providers provide the user so that organisations can directly onboard the user without aksing for details
The organisation is not required to store the users personal details and the users dont have to create an account.

OPENID Connect Participants

End user: The entity requiring authentication
Relying party: This party relies on the authorization server to provide the identity of the End User. (client in Oauth)
Identity Provider: this server that provides identity information about the end user. (auth serve in OAuth)

OpenId Connect Terminologies

Identity Token - a token that contains the user information in JSON format. The JSON is wrapped into a JWT.
The client receives the identity token, it should validate it first. the client has to validate 3 fields

iss - client must validate that the issuer of this token is the Authorization server
aud - client must validate the token is meant for it.
exp - that the token is not expired
For example
Screenshot from 2023-03-29 10-06-10.png

{
  "iss": "https://server.example.com",
  "sub": "24400320",
  "aud": "s6BhdRkqt3",
  "nonce": "n-0S6_WzA2Mj",
  "exp": 1311281970,
  "iat": 1311280970,
  "auth_time": 1311280969,
  "acr": "urn:mace:incommon:iap:silver"
}
iat - the time when the JWT was issued, must be a numericdate value
auth_time - when the end_user authentication occured. its value is a json number representing the number of seconds from 1970–01–01T0:0:0Z as measured in UTC until the date/time.
nonce - ID token request may come with a nonce request parameter to protect from replay attacks. when it is included the server will embed a nonce claim in the issued ID token with the same value of the request parameter.

SCOPE
client app must tell the identity provider about what user information is looking for using the scope field. there are 4 types of scoped defined in OpenID Connect.
email - user's email information
phone - user's phone details
profile - user's default information
address - user's address

CLAIMS
name/value pairs that contain information about a user
E.G family_name claim is used to send the family name of a user
benefit of claims is that each client knows what claims to look for to get particular user information.
either request the category of a claim by providing the scope field or it can request individual claims using the optional claims request parameter.

Screenshot from 2023-03-29 10-18-18.png

Endpoints
authorization server defines some endpoints that are used by the client to request some data.

Authorization endpoint - endpoint returns the authorization code after the user authenticates and provides their consent to the authorization server

Token endpoint - endpoint is used to exchange an authorization token with an access token and identity token. The identity token contains some basic user information.

UserInfo endpoint - UserInfo endpoint is used by the client to request user profile information that was previously granted access to. To access this information, the client needs to send an access token with the request.