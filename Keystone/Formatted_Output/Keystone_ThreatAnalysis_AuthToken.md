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

Crypto: hashlib(md5) - to hash PKI tokens

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
 -  Configuration Parameters
   
###Security Objective
 -  Integrity of the token.
 -  Authenticity of the token
 

<a name="dfd"/>
###Data Flow Diagrams 

####Token Validation Simplistic
![Image Description][1]

####Token Validation (Expanded Validate User Token from previous, Level 2, DFD)
![Image Description][3]

####Keystone Client Middleware
![Image Description][2]

Note: Token hashing algorithm is not limited to MD5 anymore, rather configurable. It coule be 
a set of hash algorithm but preferred one is the first hash algorithm.
Assume its a clean installation (no legacy token cache exist)

Validation (against revocation list ) of cached token is added (configured)

####Memcache Level 1
![Image Description][4]

Note: digest = HMAC(secrect,token+strategy, SHA386).digest
cache_key = digest[0-127]
MAC_key= digest[128-255]
Encryption_key = digest[256-385]


<a name="entry"/>
###Entry Points
Upstream Pipeline ( upstream pipe line app from which gets env including token )

HTTP request (UUID token validation, version info)

----------
<a name="asset"/>
###Assets
Full assets list is documented in url
[Asset Library][5]

8) Token
   - uuid
   - PKI (PEM, DER)

26) Configuration Parameter 
   
     -auth_uri (where to find the auth service, public api endpoint to validate token)
     -identity_uri(Identity admin api endpoint)
     -signing_cert_file_name, signing_ca_file_name, revoked_file_name
     -admin_token (going to be deprecated or service token)
     -service_user, service_password, service_tenant (for UUID token verification)
     -token_cache_enable_disable
     -token_cache
     -token_cache_time, 
     -token_revocation_list
     -token_revocation_list_cache_time
     -memchache secret key, memcache_security_strategy
     -token_bind
     -delay_auth_decision
     -include_catalog


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
>In case of token revocation, there could be inconsistency between the cached token in target service and the current state of the actual token. This happens due to a stale revocation list.

Vulnerable Component:
>Cache Management, Validating the cached token.

Counter Measures:
>By refreshing (cache) revocation list frequently, we can minimize the impact or no caching for token is another way to eliminate this threat. Deployers should consider the balance between performance and security during initialization of token cahce time, revocation list cache time and cahce enablaement feature.


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
>What if revocation list becomes stale or empty for some reason at source or at target system. Bug[1182920] shows, restarting Memcache looses token revocation list. This happens only for PKI tokens with memcache or KVS backend. An attacker can use a revoked token if Memcache server restarts.

Security Weakness:
>Lacking Integrity and data loss check of the revocation list. Do we have proper mechanism to maintain revocation list. As for bug[1182920], with memcached backend, the revoked token  list (revocation list) only lasts as long as the memcached server is up and running. Thus, if the Keystone server is restarted, all token revocations are dropped, and they will not show up in later token revocation list requests. 

Vulnerable Component:
>Memcache Backend design, data loss prevention mechanism

Counter Measures:
>Data loss prevention design.

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
>Internal attacker 

Attack Vectors:
>An internal attacker can define a large enough service catalog resulting in a 
large token which can possibly break/deny services for token users.
Bug[1186177] shows that PKI token is getting larger in V3 API. Due to limitation of WSGI MAX_HEADER_LENGTH, a large PKI token can be rejected due to limiation of WSGI request header size limit. 

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
>For UUID token in version 2.0 Identity API the user token is appended to URL of the GET request. URL is logged by many systems, potentially revealing the Token.

Security Weakness:
>Adding sensitive information in the URL is a bad practice.

Counter Measures:
>Use V3 API. 
Secure Log Management.

Extra:
>Probability: Low

>Impact: Medium

>Related Info: 

>Comments:

####AuthToken-06
Threats:
>MiTM attack during Secure channel setup

Threat Agent:
>Internet Attacker - Unauthorized

Attack Vectors:
>Fectching of Credentials (certificates to setup secure channel between Keystone Clients and Keystone Server) and using the channel to validate token or fetch revocation_list can lead to MiTM attack. A naive Keystone Client can setup a secure channel with malicious endpoint and thereby the malicious end point can provide replayed or stale  revocation_list when  a revocation_list is requested by the service (or valid result for invalid UUID token). 

Security Weakness:
>In TLS/SSL Client assume the trustworthiness of Server, in most cases, authentication of client to the
server is provided by inner mechanism e.g., token, certificate. How our token bind with transport layer,
 are they totally independent, if so , it is highly likely that MiTM is possible without proper verification 
 of server certificate at the first place. 

Counter Measures:
>Server certificate validation

Proper use of security bindings of transport protocol and authentication protocols. Best practice would be,
service binding (token is binded targeted towards a specfici service) or channel binding ( transport layer protocol is binded to authentication protocol).

Extra:
>Probability:

>Impact:

>Related Info:

>Comments:
      
####AuthToken-07
Threat:  

Threat Agent:
>

Attack Vectors:
>

Security Weakness:
>

Counter Measures:
>

Extra:
>Probability: Low

>Impact: Medium

>Related Info: 

>Comments:

####Possible Areas we can look into
1) multiple token types (uudi, PKI - pem, der) without any configurable options in Auth Token
Does attacker deliberately choose weak token creation mechanism
2) Auth_url - is it possible to forge
3) New cache lookup for a set of hash algorithm - requires more look up for same value in cache.
4) token authenticity is based on loose assumption - client, transport and all intermediaries are
   trusted.



  [1]: images/DFD_KeystoneMiddleware_Level1.png
  [2]: images/DFD_KeystoneMiddleware_ValidateUserToken_Level1.png
  [3]: images/DFD_KeystoneMiddleware_Level2.png
  [4]: images/DFD_KeystoneMiddleware_MemCache_Level1.png
  [5]: Keystone_asset_library.md
  
 

  
