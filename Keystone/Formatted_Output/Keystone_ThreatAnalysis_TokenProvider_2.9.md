Keystone Threat Modeling : Token Provider
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
Keystone has a customizable token provider. Token provider can be either UUID or PKI token. In Havana, PKI token is used as a default token provider. UUID token is 128 bit, provides a random opaque, generated using uuid4(). PKI tokens are cryptographic Message syntax (CMS) string, generated using OpenSSL. PKI tokens require signing key and cert. The cert needs to be distributed earlier to the token verifier before verifing the token. Both PKI and UUID token are bearer tokens.

####Additional Info

<a name="implementation"/>
###Implementation Overview
####Major Components
Keystone.token.providers.pki/uuid

UUID driver

PKI driver
    
####Dependent components
UUID generator = 128 bit

Crypto: CMS (Openssl)
Key size and algorithm used to sign ?

Other Kesytone  components: Token API 

Key: Signing key, Certflie

####Description
Major process:

Issue token:  Issue_v2_token, Issue_v3_token

Validate token: Validate_v2_token, Validate_v3_token

Revoke token: revoke_token 

<a name="assumption"/>
###System Assumptions (External Dependencies)
 -  Requests arriving from Token API or Token Controller are  trustworthy
   

###Security Objective
 - Generate cryptographically secure random id as token_id
 - Service Availability

<a name="dfd"/>
###Data Flow Diagrams 
####issue v2 token 
![enter image description here][1]
####issue v3 token
![enter image description here][2]
####validate v2 token
 ![enter image description here][3]

<a name="entry"/>
###Entry Points
####Token Controller
Requester to generate token (both UUID or PKI token)

####CMS (openssl)
Subprocess to perform PKI token signing. Signed token is returned from CMS.

####Cache
Token for validation can be retrieved from cache

####Persistence layer (DB):
Token creation phase data is stored in DB, validation phase data is retrieved from DB

----------
<a name="asset"/>
###Assets
Full assets list is documented in [url][4]

8) Token (Token_Ref) , Token_id, Unique_id

8.3) Issue_time

22) PKI Key

2&) Configuration (Expiry_time)

28) UUID.UUID4()


----------
<a name="threats"/>
###Threats
####TokenProvider-01
Threat: 
>Spoofing: Spoofing of UUID4 Token

Threat Agent:
>Internet attacker

Attack Vectors:
>Using weakness in UUID4 token generation (randomness), an attacker can guess a valid UUID token ID.

Security Weakness:

>UUID4 uses os.urandom to generate 128 bit randomness and encode 122 bit of it to generate a UUID4 token. 
Assuming os.urandom provides random bits, the attacker has the 2^(-122) probability to guess a UUID4 token id. 

Counter Measures:
>M2.crypt or other crypt libraries to get full 128 bit of randomness. 

Extra:
> Probability: Near Zero

> Impact: High

> Related Info:
   http://lists.openstack.org/pipermail/openstack-security/2014-February/001023.html

> Comments: This is not a feasible threat towards the component. This is reported as an informative note.


####TokenProvider-02
Threats:
>Tampering : Expiration time tampering

Threat Agent:
>Internal attacker/ wrong value assignment

Attack Vectors:
>Token expiry is determined by the configuration 'expiration' parameter. An internal attacker 
(with the permission to change conf file) can modify the 'expiration' parameter to generate long lived valid token, 
increasing the exposure possibility. Or, reduce the expiration time to a low value, causing frequent new token generation. This can lead to performance degradation and possibly DoS.  

Security Weakness:
> 

Counter Measures:
>Keystone.conf file permission limited.
Any changes to 'expiration' value should be judged against Security vs Performance. 

Extra:

>Probability: Low

>Impact: Low

>Related Info: 

>Comments: This is reported as an informative note.
  

####TokenProvider-03
Threats:
>Spoofing: Spoofing of PKI Token

Threat Agent:
>Internal Attacker

Attack Vectors:
>PKI token is created using openssl which requires signing key. The signing key is referred in the keystone.conf file.Two scenarios can happen here 1) a system user with the ability to modify the keystone.conf file can point to a tampered signing key.2) A system user with ability to read signing keyfile can can generate PKI tokens 

Security Weakness:
>  

Counter Measures:
>- keystone.conf file access control limited.
 - Signing key file access limited.
 

Extra:

>Probability:Low

>Impact:High
   
>Related Info:

>Comments: This is reported as an informative note.
   
####TokenProvider-04
Threats: 

>Denial of Service attack for PKI token

Threat Agent:

>Internet attacker

Attack Vectors:
>Depending on the setup, PKI tokens may need to sign large amount of data (with V3, including catalog) during token generation process. A large number of token signing request can lead to performance degradation of PKI signing engine. 

Security Weakness:
>Large amount of data to sign during token generation 

Counter Measures:
>Reduce token size: compress, striping of catalog

Extra:

>Probability: Low

>Impact: Medium
 
> Related Info: http://www.gossamer-threads.com/lists/openstack/dev/35240
   https://bugs.launchpad.net/ubuntu/+source/keystone/+bug/1071865

>Comments:
   
####TokenProvider-05
Threats:

>Repudiation: Accountability of Token provider

Threat Agent:
> Internal

Attack Vectors:
>An internal attacker can place a token directly in the DB/persistence layer 
without knowing which token providers has created the token. 


Security Weakness:
>The token provider does not generate any log event. 

Counter Measures:
>Token provider should generate a log event for each generated token.

Extra:
>Probability: High

>Impact: Low

>Related Info:

>Comments:
   
### Issues:

1. 

  [1]: images/DFD_Token_provider_issue_v2_token.png
  [2]: images/DFD_Token_provider_issue_v3_token.png
  [3]: images/DFD_Token_provider_validate_v2_token.png
  [4]: Keystone_asset_library.md
