Keystone threat modeling : keystonemiddleware
=========================================
### Table of contents
[System overview](#system)

[Implementation overview](#implementation)

[System assumptions](#assumption)

[Data flow diagrams](#dfd)

[Entry points](#entry)

[Assets](#asset)

[Threats](#threats)

<a name="system"/>
###System overview
####Application version
Keystonemiddleware juno

####Application description
Keystonemiddleware provides authentication and authorization features to web
services using OpenStack Identity API (keystone). It is placed in the
configuration pipeline of WSGI enabled web services (e.g.,nova, glance) to
support a common authentication protocol in use among OpenStack projects. The
middleware component acts as a proxy that intercepts HTTP calls, validate the
headers, and populates HTTP headers in the request context for use in the
downstream WSGI middleware or applications.

####Additional info
From juno release, keystonemiddleware is an independent middleware module.
Earlier, it was part of python-keystoneclient and keystone server. More
information from
http://docs.openstack.org/developer/python-keystoneclient/middlewarearchitecture.html

<a name="implementation"/>
###Implementation overview
####Major components

auth_token:
 - Verifies that incoming client requests have valid tokens by validating
   tokens with the auth service.
 - Rejects unauthenticated requests UNLESS it is in 'delay_auth_decision'
   mode, which means the final decision is delegated to the downstream WSGI
   component (usually to the OpenStack service).
 - Collects and forwards identity information based on a valid token
   such as user name, tenant, etc.

####Dependent components
1. cms:
   - Certificate signing functions
2. memcache_crypt:
   - Utilities for memcache encryption and integrity check. Encryption has a
     dependency on the *pycrypto*. If *pycrypto* is not available,
     CryptoUnavailableError will be raised.
3. hashlib:
   - To hash PKI tokens using a configured hash algorithm.The default hash
     algorithm is md5.

####Major use cases:
1. Verifies that incoming client requests have valid tokens by validating
   tokens with the auth service (UUID token).
2. Verifies that incoming client requests have valid tokens by validating
   tokens using token signing certificate (PKI token).
3. Delay authentication decision.
4. Signing certificate management.
5. Revocation list fetching.
6. Memcache management.

<a name="assumption"/>
###System assumptions
 -  Idenity API (auth service) is trustworthy.
 -  Memcache is initialized and the memcache secret key is only accessible to
    the trusted parties.
 -  The communication channel between the auth service and the
    keystonemiddleware is protected by secure tunnel protocol.

###Security Objective
 - Verify the integrity of the token and token data (authenticity,and freshness)

<a name="dfd"/>
###Data flow diagrams

####UUID token validation
![Image Description][1]

####PKI token validation
![Image Description][2]

####Delay authentication decision
![Image Description][3]

####Signing certificate management
![Image Description][4]

####Revocation list fetching
![Image Description][5]

####Cache management
![Image Description][6]


<a name="entry"/>
###Entry Points
1. Request env variables (from client request)
2. Config parameters
3. Cache
4. Certificate store
5. UUID token validation data from auth service
6. Signing and revocation_list data from auth service

<a name="asset"/>
###Assets

1. Request env parameters
   - X-Auth-Token:UUID or PKI token
2. Config parameters
   - auth_URI (identity_URI)
   - signing_cert_file_name, signing_ca_file_name, revoked_file_name
   - service username, password, service token
   - service certificate for authentication (not used)
   - token cache related configuration: token_cache_enable,
     revocation_check_on_cached_token
   - token_cache_time
   - token revocation list related configuration:token_revocation_list,
     token_revocation_list_cache_time
   - memcache related configuration: secret key, memcache_security_strategy
   - token_bind
   - delay_auth_decision
   - http_max_retries
3. Cache
   - token_id, token_data, per object secret key
4. Certificate Store
5. UUID token validation data from auth service (Identity API)
   - response code, data related to token in HTTP headers
6. Signing and revocation list data from auth service (Identity API)
7. Internal important env variables
   - X-Identity-Status
   - X-Service-Catalog
   - X-Roles
   - X-Service-Roles
   - keystone.token_info

----------
<a name="threats"/>
###Threats

>
####Spoofing: unauthorized access to the target service (OR)
- S1 Attacker populates authentication headers in request env and
     keystonemiddleware fails to remove authentication headers (P:Zero)
- S2 Validate UUID token against a compromised Identity API (P:Low)
     - S2.1 Identity API is compromised
     - S2.2 Keystonemiddleware do not verify authenticity of the Identity API
            endpoint e.g., server certificate check turned off (P:Low)
- S3 Delay_auth_request flag is set and delay_auth_module is not trusted (P:Zero)
- S4 A revoked token can be used as a valid token (P:Low)
     - S4.1 check_revocation_for_cached token flag is not set and a revoked
             token which is in cache is used to access target service (P:Low)
     - S4.2 Compromised revocation list (P:Medium)
            - S4.2.1 Revocation list provider (Identity API) is compromised
                      (P:Low)
                     - S4.2.1. Signing key for revocation list is compromised
            - S4.2.2 Revoked token is not updated/added to the revocation
                      list in provider side (P:Low)
                     - S4.2.2.1 Backend failure cause earlier token revocation
                                history removed (P:Low)
                     - S4.2.2.2 Revocation_list cache is not updated
                                when token revocation happens (P:Zero)

    S4.2. A stale revocation_list is sent by a network attacker(P:Low)
           S4.2.1. MiTM  in the communication channel (AND)
           S4.2.2. Keystone middleware cannot check the freshness of the
                   revocation_list (P:Low)
     S4.3. A token is revoked when revocation_list is in keystonemiddleware
           cache and before revocation_list_cache_time out happens (P:Low)
 S5. Keystonemiddleware memcache is compromised (P:Low)
     S5.1. Attacker reads a token from memcache and use it on behalf of the
           user. This can happen for integrity protected token in memcache
           (P:Low)
     S5.2. Attacker places compromised token in memcache (P:Zero)
           S5.2.1 Cache secret key is compromised
 S6. Validate PKI token against compromised certificate (P:Medium)
     S6.1. Certificate authority is compromised (P:Low)
     S6.2. Signing certificate is compromised (P:Medium)
           S6.2.1 Signing certificate revocation mechanism does not exist in
                  the keystonemiddleware (AND)
           S6.2.1 Revoked certificate in use(P:Medium)
     S6.3. Signing certificate store is compromised in keystonemiddleware
           (P:Zero)
           S6.3.1. Exploitation of certificate store access permission
     S6.4. A stale certificate is sent by the network attacker (P:Low)
           S6.4.1. MiTM in the communication channel (AND)
           S6.4.2. Keystonemiddleware has no root of trust to verify the
                   authenticity of the signing certificate (P: Low)

####Tampering of data: modification of sensitive data (OR)
>T1. Tampering of token data in memcache (P: Zero)
 T2. Tampering of Auth headers in the request env (P:zero)

####Repudiation:
>R1. The client cannot be held responsible for a request (P:Low)
     R1.1. A requester can impersonate as the owner of the token due to the
           nature of the bearer token. No signing of the presenter exist in the
           request (P:Low)
 R2. Keystonemiddleware cannot be held responsible for its action in setting
     auth headers (P:Zero)
 R3. Delay auth modules cannot be held responsible for its action (P:Zero)

####Information disclosure: exposure of sensitive data (OR)
>I1. Token data is read from the memcache store (P:Low)
 I2. Sensitive data is read (e.g., service token, service password, cache secret
     key) from the configuration file due to lack of access control and
     plain text format.

####Denial of Service (OR)
>D1. DoS attack on keystonemiddleware (P:Low)
     D1.1. Attacker sends excessive number of invalid PKI tokens causing time
           consuming signature verification
     D1.2. Low token_cache_time causes frequent token verification
 D2. Keystonemiddleware is used to create DoS attack on Identity API (P:Medium)
     D2.1. An error in PKI token verification causes fetching of signing
           certificate from the Identity API. This can cause overload on
           Identity API (P: Medium)
     D2.2. Attacker sends many random UUID tokens for verification causing many
           token validation request sent to the Identity API (P:Medium)
     D2.3. Http_max_retries parameter is set to high value causing many requests
           send towards a non responsive Identity API (P:Low)
     D2.4. Low token_cache_time causes token fetching frequently (P:Low)
     D2.5. Low revocation_list_cache_time causes frequent revocation list
           fetching (P:Low)
 D3. Keystonemiddleware is used to create DoS on memcache (P:zero)
     D3.1. Low token_cache_time and low revocation_list_cache time causes more
           cache write operation on memcache (P:Zero)

####Elevation of privileges (OR)


####Detailed description of major threats
#####Keystonemiddleware-01
Threat:
>Use revoked tokens to access a service.

Threat agent:
>Internet attacker unauthorized

Attack Vectors:
>If a token is in cache, depending on the configuration setup,
keystonemiddleware returns the token data without verifying the freshness of the
cached token data. To mitigate this, configuration allows a cached token to be
verified against a revocation_list. However, a stale revocation_list in cache
causes revoked tokens to access the target service.

Security Weakness:
>In case of token revocation, there can be inconsistency between the cached
token and the current state of the actual token. The same is true for cached
revocation_list and the current state of the actual revocation list.

Counter Measures:
> Set cache_revocation_for cached to verify the cached token against the
revocation list. Set low value for revocation_list cache time. Deployers should
balance between consistency and performance of the system.

Extra:
>Probability: Low
>Impact: High
>Related Info:
https://bugs.launchpad.net/python-keystoneclient/+bug/1287301


  [1]: images/DFD_KeystoneMiddleware_validate_UUID_token.png
  [2]: images/DFD_KeystoneMiddleware_validate_PKI_token.png
  [3]: images/DFD_KeystoneMiddleware_delay_auth.png
  [4]: images/DFD_KeystoneMiddleware_CertificateManagement.png
  [5]: images/DFD_KeystoneMiddleware_RevocationListFetch.png
  [6]: images/DFD_KeystoneMiddleware_Memcache.png
