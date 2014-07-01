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
The Keystone Auth Token is a middleware that can be plugged into a WSGI pipeline to perform authentication with the central Keystone server. An authentication middleware component is a proxy that intercepts HTTP calls from clients, validate the header, and populates HTTP headers in the request context for other WSGI middleware or applications to use.


####Additional Info

<a name="implementation"/>
###Implementation Overview

Implemented as as WSGI middleware

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
 -  Memcache is initialized and the memcache secret key is accessible only by the Client.
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
>Probability: Low

>Impact: High

>Related Info: 
https://bugs.launchpad.net/python-keystoneclient/+bug/1287301

>Comments: 

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
>Probability: Low

>Impact: Medium

>Related Info: 
https://bugs.launchpad.net/keystone/+bug/1182920
https://blueprints.launchpad.net/keystone/+spec/revocation-backend
https://etherpad.openstack.org/p/keystone-revocation-events

>Comments:

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
>Probability: Very Low

>Impact: Medium

>Related Info: 

>Comments:

####AuthToken-04

Threat: Service unavailablity

Threat Agent:
>Internet attacker Authorized.

Attack Vectors:
>The PKI token is getting larger in V3 API. Due to limitation of WSGI MAX_HEADER_LENGTH, a large PKI token can be rejected due to limiation of WSGI request header size limit. 

Security Weakness:
>

Counter Measures:
>PKI Token with no catalog information. Increase the size of MAX_HEADER_LENGTH (Any security implication?)

Extra:
>Probability: 

>Impact: 

>Related Info: 
https://bugs.launchpad.net/python-keystoneclient/+bug/1186177

>Comments:

####AuthToken-05
Threat: Information Diclosure. 
Threat Agent:
>Internal Attacker.

Attack Vectors:
>For UUID token in version 2.0 the user token is appended to the GET request. The same is also for PKI token, if any service calls PKI token for validation using GET call.

Security Weakness:
>Log Analysis from Keystone Middleware requests could reveal the user token.


Counter Measures:
>Use V3 API. Secure Log Management.

Extra:
>Probability: Medium

>Impact: High

>Related Info: 

>Comments:

####AuthToken-06
Threats:
>MiTM attack during Secure channel setup

Threat Agent:
>Internet Attacker - Unauthorized

Attack Vectors:
>Fectching of Credentials (certificates to setup secure channel between Keystone Clients and Keystone Server) and using the channel to validate token or fetch revocation_list can lead to MiTM attack. A naive Keystone Client can setup a secure channel with malicious endpoint and thereby the malicious can provide replayed or stale  revocation_list when  a revocation_list is requested by the service (or valid result for invalid UUID token). 

Security Weakness:
>In case of SSL endpoint, MiTM in the SSL is possible as token is no way bind to transport protocol. using this vulnerability it can replay an old revocation_list to the service
  

Counter Measures:
>SSL tunnel, server certificate validation
Binding of message with transport security.

Extra:
>Probability:

>Impact:

>Related Info:

>Comments:
     Link to Bug/mailing list or Tracking 

  [1]: images/DFD_KeystoneMiddleware_Level1.png
  [2]: images/DFD_KeystoneMiddleware_ValidateUserToken_Level1.png
  [3]: images/DFD_KeystoneMiddleware_Level2.png
  [4]: images/DFD_KeystoneMiddleware_MemCache_Level1.png
  [5]: Keystone_asset_library.md
  
 

  
