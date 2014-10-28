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
![Image Description][x]

####external.KerberosDomain Auth Plugin
![Image Description][x]

####external.ExternalDefault Auth Plugin
![Image Description][x]

####external.ExternalDomain Auth Plugin
![Image Description][x]

####external.LegacyDomain Auth Plugin
![Image Description][x]

####external.LegacyDefaultDomain Auth Plugin
![Image Description][x]

<a name="entry"/>
###Entry Points
Upstream Pipeline ( upstream pipe line app from which gets env containing REMOTE_USER, REMOTE_DOMAIN  )

HTTP request (UUID token validation, version info)

----------
<a name="asset"/>
###Assets
Full assets list is documented in url
[Asset Library][10]

1) User
5) Domain


----------
<a name="threats"/>
###Threats
####ExternalAuth-01
Threat: 
>Spoofing: REMOTE_USER, REMOTE_DOMAIN modified by middleware.

Threat Agent:
> Authorized System User

Attack Vectors:
>Configuration of the middleware can modify critical REMOTE_USER and REMOTE_DOMAIN environment variables allowing spoofing the user that the web server authenticated.

Security Weakness:
>Keystone auth plugin cannot verify who authenticated the user and if REMOTE_USER or REMOTE_DOMAIN were modified after authentication.

Vulnerable Component:
>All external auth plugins

Counter Measures:
>Validate middleware and web server configuration.

Extra:
>Probability: Low

>Impact: High

>Related Info: 

>Comments: 


  [1]: images/DFD_External_Auth_Plugin.png
  [10]: Keystone_asset_library.md
  
 
