Keystone Threat Modeling : External Auth
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
Keystone Master
   
####Application Description
Keystone External Auth allows Keystone to authenticate a user based on authentication performed by upstream components such as the paste middleware or web server.

####Additional Info

<a name="implementation"/>
###Implementation Overview

Implemented as a Keystone authentication plugin.

####Major Components

* Keystone external.Domain Auth Plugin
* Keystone external.DefaultDomain Auth Plugin
* Keystone external.KerberosDomain Auth Plugin
* Keystone external.ExternalDefault Auth Plugin
* Keystone external.ExternalDomain Auth Plugin
* Keystone external.LegacyDomain Auth Plugin
* Keystone external.LegacyDefaultDomain Auth Plugin

####Dependent components

* Web Server
* Paste Middleware

####Description

Major process:

Get REMOTE_USER and REMOTE_DOMAIN from reqest environment. Lookup user and domain reference. Return to caller if successful.

<a name="assumption"/>

###System Assumptions (External Dependencies)
 - Web server or middleware correctly authenticates user and sets REMOTE_USER and REMOTE_DOMAIN.
 - 
   
###Security Objective
 -  Validate user and domain are valid.
 - validate bindings???

<a name="dfd"/>
###Data Flow Diagrams 

####external Auth Plugin
This needs to be broken up into the separate flows below.
![external auth DFD flow][1]

####external.Domain Auth Plugin
![Image Description][x]

####external.DefaultDomain Auth Plugin
![Image Description][2]

####external.KerberosDomain Auth Plugin
![Image Description][3]

####external.ExternalDefault Auth Plugin
![Image Description][4]

####external.ExternalDomain Auth Plugin
![Image Description][5]

####external.LegacyDomain Auth Plugin
![Image Description][6]

####external.LegacyDefaultDomain Auth Plugin
![Image Description][7]

<a name="entry"/>
###Entry Points
Upstream Pipeline ( upstream pipe line app from which gets env containing REMOTE_USER, REMOTE_DOMAIN  )

HTTP request (UUID token validation, version info)

----------
<a name="asset"/>
###Assets
Full assets list is documented in url
[Asset Library][5]

1) User
5) Domain


----------
<a name="threats"/>
###Threats
####ExternalAuth-01
Threat: 
>Spoofing: REMOTE_USER, REMOTE_DOMAIN modified by middleware.

Threat Agent:
> Authorized User

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


  [1]: images/DFD_KeystoneMiddleware_Level1.png
  [2]: images/DFD_KeystoneMiddleware_ValidateUserToken_Level1.png
  [3]: images/DFD_KeystoneMiddleware_Level2.png
  [4]: images/DFD_KeystoneMiddleware_MemCache_Level1.png
  [5]: Keystone_asset_library.md
  
 

  
