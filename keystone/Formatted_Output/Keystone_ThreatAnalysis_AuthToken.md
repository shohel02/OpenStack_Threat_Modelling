

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

  [1]: images/DFD_KeystoneMiddleware_Level1.png
  [2]: images/DFD_KeystoneMiddleware_ValidateUserToken_Level1.png
  [3]: images/DFD_KeystoneMiddleware_Level2.png
  [4]: images/DFD_KeystoneMiddleware_MemCache_Level1.png
  
 

  