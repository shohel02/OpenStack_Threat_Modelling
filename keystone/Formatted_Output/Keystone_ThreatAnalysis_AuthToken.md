Keystone Threat Modeling : Auth Token
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
The Keystone middleware architecture supports a common authentication protocol for all OpenStack projects.  By using keystone as a common authentication and authorization mechanisms, the OpenStack project can plug in to existing authentication and authorization systems.

OpenStack is using a token-based mechanism for authentication and authorization. An authentication middleware component is a proxy that intercepts HTTP calls from clients, validate the header, and populates HTTP headers in the request context for other WSGI middleware or applications to use


####Additional Info

<a name="implementation"/>
###Implementation Overview

####Major Components

Keystone Auth token 

####Dependent components
Json utilties


Crypto: hashlib(md5)

Signing certificate, certificate authority

Token Caching (memcache)

####Description

Major process:

Get user token, Validate user token, Build user header, Add user header, Data to service.

<a name="assumption"/>

###System Assumptions (External Dependencies)
 -  Token service is trustworthy.
 -  Memcache is initialized and the memcache secret key is available.
 -  The communication channel is secure.
 -  Configuration Parameters: Token Expiry time and Token Issue time.
   
###Security Objective
 -  Validation of the token.
 

<a name="dfd"/>
###Data Flow Diagrams 

####Token Validation Simplistic
![Image Description][1]

####Keystone Client Middleware
![Image Description][2]

####Token Validation (Expanded Validate User Token from previous, Level 2, DFD)
![Image Description][3]

####Memcache Level 1
![Image Description][4]

<a name="entry"/>
###Entry Points

####Public Port
SSL protected port, used to access the keystone server. External requests come and return through this port. Default 5000. If you plan to use SSL proxy, it could be different.

####Admin Port
SSL protected port, used to access the keystone server. External requests come and return through this port. Default 35357. If you plan to use SSL proxy, it could be different. Used only in V2 API

----------
<a name="asset"/>
###Assets
Full assets list is documented in url

----------
<a name="threats"/>
###Threats
####AuthToken-01
Threat: 
>Spoofing: Unauthorized access to target service.

Threat Agent:
>Internet attacker Unauthorized.

Attack Vectors:
>If token  is found in cache, the target service returns the token data without validating the cached token.

Security Weakness:
>In case of token revocation there could be inconsistency between the cached token in target service and the current state of the actual token. Similarly, there could be inconsitency in terms of token data related to a token present in the cache and the token present in the database.

Vulnerable Component:
>Cache Management, Validating the cached token.

Counter Measures:
> 

Extra:
> Probability: 

> Impact: 

> Related Info:

> Comments: 

####AuthToken-02
Threat: 

Threat Agent:
>Internet attacker Unauthorized.

Attack Vectors:
>Restarting Memcache looses token revocation list. This happens in case of memcache and KVS backend only for PKI tokens.

Security Weakness:
>With the memcached backend tokens, the revoked token  list only lasts as long as the memcached server is up and running. Thus, if the Keystone server is restarted, all token revocations are dropped, and they will not show up in later token revocation list requests. 

Vulnerable Component:
>Memcache Backend design.

Counter Measures:
> 

Extra:
> Probability: 

> Impact: 

> Related Info: https://bugs.launchpad.net/keystone/+bug/1182920

> Comments:

####AuthToken-03

Threat: Denial of Service.

Threat Agent:
>Internet attacker Unauthorized.

Attack Vectors:
>Consistent spoofing of UUID or PKI token.

Security Weakness:
>Attacker consistently sends UUID and PKI token 

Vulnerable Component:
>Rate limiting/ blocking

Counter Measures:
> 

Extra:
> Probability: 

> Impact: 

> Related Info: 

> Comments:

####AuthToken-04

Threat:Denial of Service

Threat Agent:
>Internet attacker Authorized.

Attack Vectors:
>By sending many (large PKI token) legitimate token validation request (difficult)

Security Weakness:
>

Vulnerable Component:
>

Counter Measures:
> 

Extra:
> Probability: 

> Impact: 

> Related Info: 

> Comments:

####AuthToken-05
Threat: Information Diclosure. 
Threat Agent:
>Internal Attacker.

Attack Vectors:
>For UUID token in version 2.0 the user token is appended to the GET request. The same is also for PKI token, if any service calls PKI token for validation using GET call.

Security Weakness:
>Log Analysis from Keystone Middleware requests could reveal the user token.

Vulnerable Component:
>API version 2.0 GET request.

Counter Measures:
> 

Extra:
> Probability: 

> Impact: 

> Related Info: 

> Comments:

  [1]: images/DFD_KeystoneMiddleware_Level1.png
  [2]: images/DFD_KeystoneMiddleware_ValidateUserToken_Level1.png
  [3]: images/DFD_KeystoneMiddleware_Level2.png
  [4]: images/DFD_KeystoneMiddleware_MemCache_Level1.png
  
 

  
