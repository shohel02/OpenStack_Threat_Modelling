Keystone Threat Modeling : Token Controller (v2.0)
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
The V2 Token Controller routes and glues requests from router to the TOKEN API service. It performs filtering on incoming and outgoing requests.  Authenticated users are issued bearer token.

####Additional Info
V2 API for Token Operation. Request flows from Token controller to Token API and Token provider API
  

<a name="implementation"/>
###Implementation Overview
####Major Components
   Keystone/token/controllers.py

####Dependent components
 Keystone/token/routers.py
  
 TOKEN API, IDENTITY API, TOKEN PROVIDER API, TRUST API, POLICY


####Description
Major process:

  POST /token authenticate

  GET  /token/token_id validate
  
  HEAD /token/token_id validate_token_head
 
  DELETE /token/token_id delete
  
  GET /token/revoked revocation_list
  
  GET /token/token_id/endpoints endpoints  (not covered)

<a name="assumption"/>
###System Assumptions (External Dependencies)

 - The communication channel between end user (client) and Keystone sever is SSL/TLS protected.
 - Only V2.0 API is covered in this doc.

###Security Objective

1. Issuance of integrity protected Token
2. Validate the integrity of Token
3. Avalability of service
4. Authentication of user (desired property is mutual authentication) 
5.	Issuance of integrity protected token revocation list 
6. Auditablity of requset/response
7.	User data integrity check 
.. * username, tenant, domain, token_id length check
.. * whitelist/black list check




<a name="dfd"/>
###Data Flow Diagrams 
####Authenticate
![enter image description here][1]
####Validate Token
![enter image description here][2]
####Delete Token
![enter image description here][3]
####Revocation List
![enter image description here][4]
<a name="entry"/>
###Entry Points
####Name ID-01: Router
#####Description
Request from upper pipeline.
#####Accessible To
8) Keystone Process user

10) System admin 

####Name ID-02: Internal Services
#####Description

#####Accessible To
8) Keystone Process user

10) System admin 

####Name ID-03: Configuration
#####Description

#####Accessible To
8) Keystone Process user

10) System admin 



----------
<a name="asset"/>
###Assets
Full assets list is documented in [url][5]

(1) User (Username, UserId, user_ref)

(1.1) User Secrect ( Password, Token)

(2) Project

(8) Token (Token_id, Unique_id)

(8.1) Revocation_list

(22) PKI sign key

(35) context

(27) REMOTE_USER

(26) Configuration ( expiry_time ,MAX_PASSWORD_LENGTH, MAX_PARAM_SIZE)


----------
###TokenController-001
Threats:
> Spoofing of password 

Threat Agent:
> 

Attack Vectors:
>Using attack methods: password guessing, dictionary attack or brute force on password, audit log montiroing

Security Weakness:
>Weakness in password management and rate limiting. Currently, keystone has no control mechanism to maintain password strength, and to maintain password policy
passwords are send in clear text.

Counter Measures:
> 

Extra:
> Probability:

> Impact:

> Related Info:

> Comments:
     Link to Bug/mailing list or Tracking 
     
###TokenController-002
Threats:
> Denial of service through authentication request flooding


Threat Agent:
> 

Attack Vectors:
> By flooding authentication request, the token router and the DB can become unavailable.

Security Weakness:
> Each request will hit the DB to find a  user ref for a user_id/username. Assuming user_id/username is easy to guess/Known, the DB hit will be successful leading to the expensive crypto matching for password authentication. This could potentially lead to system unavailability.
Weakness in ratelimiting, Weakness in input filtering , current check only consists of length check, no whitelisting check (?)

Counter Measures:
>  Rate Limiting, White listing

Extra:
>  Probability:

>   Impact:

>   Related Info:

>   Comments:
     Link to Bug/mailing list or Tracking 


####TokenController-003
Threats:
> Threats from configuration params.

Threat Agent:
> Internal Attacker

Attack Vectors:
>  Internal attacker abuses the configuration parameter (e.g., expiry time, 
password length, max param length.)
max_param_length field can be abused to bypass the initial input filtering on any input (e.g., User_id length), causing expensive query hit to the dB
A malicious/unthoughtful admin sets larger expiry_time, leading to a long lived token
A malicious admin/unthoughtful sets larger password length bad input filtering and unwanted DB hit.

Security Weakness:
> 

Counter Measures:
> 

Extra:
>  Probability:

>   Impact:

>   Related Info:

>   Comments:
     Link to Bug/mailing list or Tracking 
     
####TokenController-004
Threats:
> Spoofing token_id or unique_id generation using MD5 (currently it is possible to 
configure the algorithm, so this avoidable)

Threat Agent:
> 

Attack Vectors:
>  MD5 is the default mechanism to generate unique ids from PKI token.
In default policy setup, it is possible for a malicious service user to get the PKI token by a validate call using MD5 of the PKI token. A malicious service user can use the weakness of MD5 (to guess a forged token id) to get PKI token for another user.

Security Weakness:
>  MD5 for token_id/unique id generation 

Counter Measures:
> 

Extra:
>  Probability:

>   Impact:

>   Related Info:
This is changing in latest releases

>   Comments:
    https://bugs.launchpad.net/keystone/+bug/1174499 
     
####TokenController-005
Threats:
> Unauthorized access to service (Keyston-manage token flush,
takes care this issue).

Threat Agent:
> Internet attacker unauthorized, authorized.

Attack Vectors:
> PKI tokens are checked against revocation list. If a token is revoked, that information 
is updated in the revocation list. Revocation list consists of disblaed tokens. Now, Keystone
also has token cleaning operation to clean the DB for all disabled token. If a disabled token
is cleaned before a PKI token expiration time, the attacker can use the cleaning process 
and use a revoked token to access a target service.

Security Weakness:
> Security management and Token deletion mechanism 

Counter Measures:
> 

Extra:
>  Probability:

>   Impact:

>   Related Info:

>   Comments:

   
###Issues:

Authenticate:


1. Authenticate_local : password in clear text, weakness of user/password mechanism; what if multiple authentication request coming from same user or different user... rate limiting
- Out put: scoped token - how much scoped they are -- only user, tenant binding possible
- Out put: unscoped token - what issues it can cause. 

2. Authenticate_token: unscoped token+trust (both are unscoped, which one has broader unscope).

3. Authenticate External: What data is passed by external auth (or data about user is always in local storage only
auth is external. At least that is how it looks from the code). 

4. No mention of which authentication mechanism is used for authentication... is it necessary.


validate Token:

1. Token id in url ???
2. what does belongs_to provide?

Revocation List:

1. Revocation list signing and Token signing using the same key.
2. Revocation list also contains UUID token, although UUID token is validated by direct call
3. Lifetime of revocation list (short time is prefeered due to the high value asset).
4. What are the freshness guarantee of a revocation list (fetch time, signing time, do we check it during use)






  [1]: images/DFD_Token_CONTROLLER_authenticate_v2.png
  [2]: images/DFD_Token_CONTROLLER_validate_v2.png
  [3]: images/DFD_Token_CONTROLLER_delete_v2.png
  [4]: images/DFD_Token_CONTROLLER_revoke_list.png
  [5]: Keystone_asset_library.md
