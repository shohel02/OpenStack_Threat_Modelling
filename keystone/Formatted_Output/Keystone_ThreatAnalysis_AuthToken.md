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

OpenStack is using a token-based mechanism for authentication and authorization. An authentication middleware component is a proxy that intercepts HTTP calls from clients, validate the header, and populates HTTP headers in the request context for other WSGI middleware or applications to use.


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

####Upstream Pipeline

####WSGI Server
----------
<a name="asset"/>
###Assets
Full assets list is documented in url
[Asset Library][5]

1) User

8) Token

8.1) Revocation List

22) PKI signing cert

26) Configuration Parameter (Token Cache time, revocation list cache time)


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
A recent modification allowed checking chached token against revocation list. However, the revocation list itself
can be cached in the target system.

Security Weakness:
>In case of token revocation, there could be inconsistency between the cached token in target service and the current state of the actual token. Thishappens due to a stale revocation list.

Vulnerable Component:
>Cache Management, Validating the cached token.

Counter Measures:
>By refreshing (cache) revocation list frequently, we can minimize the impact or zero cache time for revocation list
can provide the consistency of token state between token source and target system. Deployers should consider the balace between performance and security during initialization of token cahce time and revocation list cache time.


Extra:
> Probability: Low

> Impact: High

> Related Info: 
https://bugs.launchpad.net/python-keystoneclient/+bug/1287301

> Comments: 

####AuthToken-02
Threat: 

Threat Agent:
>Internet attacker Unauthorized.

Attack Vectors:
>Restarting Memcache looses token revocation list. This happens only for PKI tokens with memcache or KVS backend. An attacker can use a revoked token if Memcache server restarts.

Security Weakness:
>With memcached backend, the revoked token  list (revocation list) only lasts as long as the memcached server is up and running. Thus, if the Keystone server is restarted, all token revocations are dropped, and they will not show up in later token revocation list requests. 

Vulnerable Component:
>Memcache Backend design.

Counter Measures:
>Way around is described in See Related Info.

Extra:
> Probability: Low

> Impact: Medium

> Related Info: 
https://bugs.launchpad.net/keystone/+bug/1182920
https://blueprints.launchpad.net/keystone/+spec/revocation-backend
https://etherpad.openstack.org/p/keystone-revocation-events

> Comments:

####AuthToken-03

Threat: Denial of Service.

Threat Agent:
>Internet attacker Unauthorized.

Attack Vectors:
>Consistent spoofing of UUID or PKI token.

Security Weakness:
>Attacker consistently sends UUID and PKI token (malformed) to the target system.

Counter Measures:
>Deployer can have Rate limiting/ blocking to counter this.

Extra:
> Probability: Very Low

> Impact: Medium

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
  [5]: Keystone_asset_library.md
  
 

  
