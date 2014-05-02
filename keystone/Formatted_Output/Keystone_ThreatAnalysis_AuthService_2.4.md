Keystone Threat Modeling : Auth Controller V3
=========================================
### Table of contents

[System Overview](#system)

[Implementation Overview](#implementation)

[System Assumptions](#assumption)

[Data Flow Diagrams](#dfd)

[Entry Points](#entry)

[Assets](#asset)

[Threats](#threats)


<a name="system"/>
###System Overview
####Application version
Keystone Havana Stable Release
   
####Application Description
The V3 Auth Controller routes and glues the requests from service_v3 middleware to the appropriate internal services. It also performs filtering on incoming and outgoing requests.  Authenticated users are issued bearer token.

####Additional Info
  

<a name="implementation"/>
###Implementation Overview
####Major Components
Keystone/auth /controllers.py
   
####Dependent components
Keystone/auth/routers.py

Keystone/auth/plugins/{authentication_plugin}

TOKEN API, IDENTITY API, TOKEN PROVIDER API, TRUST API, POLICY
####Description
Each authentication requests is a POST request, must include user credential, authentication method, and optionally ‘scope’. The scope is either project or domain, not both. Scope is also identifiable from trust_id. The authentication token is returned in X-Subject-Token header

POST /auth/tokens authenticate_for_token

GET validates the token. Only permitted roles (defined in policy.json) can validate a token

GET  /auth/tokens validate_token

HEAD /auth/tokens check_token

Delete token disables a token by changing the ‘enable’ flag

DELETE /auth/tokens revoke_token

 More info: http://api.openstack.org/api-ref-identity.html

<a name="assumption"/>
###System Assumptions (External Dependencies)
 - The communication channel between end user and keystone is SSL protected.
 - Only V3.0 API is covered in this doc.

###Security Objective
-	Authentication of user 
-	Validate the authenticity of the token


<a name="dfd"/>
###Data Flow Diagrams 
####Top level
![enter image description here][1]
####Authenticate
![enter image description here][2]
####Validate Token
![enter image description here][3]
####Check Token
![enter image description here][4]
####Delete Token
![enter image description here][5]
 
<a name="entry"/>
###Entry Points
####Name ID-01: S3_router
#####Description
Request from upper pipeline.
#####Accessible To
8) Keystone Process user

10)System admin 

####Name ID-02: External Identity Provider
#####Description
Identity Request resolved by external Provider. 
Set REMOTE_USER variable
#####Accessible To
8) Keystone Process user 

10)System admin

13)External Identity Provider User

####Name ID-03: Other Internal Services
#####Description
IDENTITY API, TOKEN PROVIDER API, TOKEN  API


----------
<a name="asset"/>
###Assets
Full assets list is documented in url
[Asset Library][6]

1) User

1.1) User Secret (password, Token)

8) Token

8.2.) Scope (project, Domain, Trust)

26) Configuration (Expiry_time)

27) REMOTE_USER

35) Context 

36) Auth Mecahnsim (Auth Chaining)

----------
####AuthController -001
Threats:
> Spoofing: Unauthorized access

Threat Agent:
> Internet Attacker - Unauthorized

Attack Vectors:
> REMOTE_USER asset context exploited

Security Weakness:
> Context generation weakness. No credible one found, but possible exploitation area. Need to check, we can pass the REMOTE_USER in query string. Can it escape somehow the query string parameter in context? Special checks: no one tries to flatten the context. Is REMOTE_USER is set by other 
source other than authentication purpose? Not all external authentication mechanism do set
REMOTE_USER variable , what to expect for such cases.

Counter Measures:
> Follow best practices

Extra:
>  Probability: Zero (current state)

>   Impact: High

>   Related Info:

>   Comments:
     1. http://lists.openstack.org/pipermail/openstack-dev/2013-October/017886.html
     2.http://www.freeipa.org/page/Environment_Variables#X.509_Authentication
     

####AuthController-002
Threats:
> Spoofing: 

Threat Agent:
> Internet Attacker-authorized or unauthorized,

Attack Vectors:
> Exploiting the weakness of supported authentication mechanism.

Security Weakness:
> V3 support multiple authentication mechanism. Support for both weak and strong authentication at the same time to get authentication token could resulted in a threat in which an attacker could 
use the weaker authentication mechanism to get a TOKEN. An attacker will always choose the lower grade authentication mechanism to attack the target.  For example, supporting password and one time password both at the same time. The attacker will choose the lower strength auth method to achieve his goal. Weakness could generate from supporting multiple authentication.

Counter Measures:
> Security best practice to decide, resources available based on authentication method strength

Extra:
>  Probability: Low

>   Impact: High

>   Related Info:

>   Comments:

####AuthController-003 
> Threats: DoS using Token Validation 

Threat Agent:
> Internet Attacker - Unauthorized

Attack Vectors:
> Token validation request for UUID tokens

Security Weakness:
> Currently there is no white listing check on TokenID (UUID Token) during token validation. All tokenIDs are considered in correct form, which could result in expensive DB lookout to check the correctness of the Token_ID.

Counter Measures:
> Rate Limiting

Extra:
>  Probability: Medium

>   Impact: Medium

>   Related Info:

>   Comments:

####AuthController-004
Threats:
> Spoofing: Unauthorized access using Bearer Token Weakness

Threat Agent:
> 

Attack Vectors:
> 

Security Weakness:
> 

Counter Measures:
> 

Extra:
>  Probability: 

>   Impact: 

>   Related Info:

>   Comments:


####AuthController-005
Threats:
> Spoofing: Unauthorized access using the weakness
default auth mechanisms : User/Password authentication

Threat Agent:
> 

Attack Vectors:
> 

Security Weakness:
> 

Counter Measures:
> 

Extra:
>  Probability: 

>   Impact:

>   Related Info:

>   Comments:

####AuthController-006
Threats:
> Spoofing/ Elevation of Priviledge: Unauthorized access using the weakness
default auth mechanisms : token authentication

Threat Agent:
> 

Attack Vectors:
> 

Security Weakness:
> currently a unscoped token can be scoped and scoped token can be unscoped. This is true for both V2 and V3. An malicious entity in possession of a scoped token can create a new token with a changed scope ( a new token changing the project role to which the token owner is a member)

Counter Measures:
> 

Extra:
>  Probability: 

>   Impact: 

>   Related Info:

>   Comments:

####AuthController-007
Threats:
> Spoofing: unauthorized access: 
Impedance mismatch among several authentication mechanism context

Threat Agent:
> 

Attack Vectors:
> 

Security Weakness:
> Keystone currently supports multiple authentication mechanism and authentication chaining. The granularity and scope of each authentication mechanism can be different.

Counter Measures:
> 

Extra:
>  Probability: Medium

>   Impact: 

>   Related Info:

>   Comments:
https://www.isecpartners.com/media/11958/common%20flaws%20of%20distributed%20identity%20and%20authentication%20systems.pdf

####AuthController-007
Threats:
> Spoofing/Elevation of Priviledges: Token Scoping is too large

Threat Agent:
> 

Attack Vectors:
> 

Security Weakness:
> Currently, Token is binded to user, project and role, not with the 
endpoint. A single token is valid for all services. Besides, a scoped
token can be used to get an unscoped token and later on scoped token 
makes all user asset vulnerable if it get exposed in any place.

Counter Measures:
> 

Extra:
>  Probability: 

>   Impact: 

>   Related Info:

>   Comments:





  [1]: images/DFD_Auth_Router.png
  [2]: images/DFD_AUTH_CONTROLLER_authenticate_v3.png
  [3]: images/DFD_AUTH_CONTROLLER_Validate_V3.png
  [4]: images/DFD_AUTH_CONTROLLER_Check_V3.png
  [5]: images/DFD_AUTH_CONTROLLER_revoke_V3.png
  [6]: Keystone_asset_library.md
