Keystone Threat Modeling : Credential Service
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
Keystone Havana Stable Release.
   
####Application Description
The Keystone Credential Service falls under the Identity service. http://api.openstack.org/api-ref-identity.html
This document is in continuation with the Threat Modeling of Keystone Identity and Assignment Driver.

####Additional Info


<a name="implementation"/>
###Implementation Overview
####Major Components

- Keystone Credential Controller.

- Credential Drivers (example SQL).

####Dependent components

Keystone Policy Engine.

####Description

<a name="assumption"/>

###System Assumptions (External Dependencies)
 -  The default driver for Keystone Credential Service is SQL.

###Security Objective
 - Provide authentic CRUD operation for Credential Services.
 

<a name="dfd"/>
###Data Flow Diagrams 
####Create Credentials
![enter image description here][1]
####UpdateCredentials
![enter image description here][2]
####DeleteCredentials
 ![enter image description here][3]

<a name="entry"/>
###Entry Points

####Public Port
SSL protected port, used to send request and receive response from  the Keystone server. Default port is 5000. 

####Persistence layer (DB):
Token creation phase data is stored in DB, validation phase data is retrieved from DB.

----------
<a name="asset"/>
###Assets
Full assets list is documented in url
[Asset Library][4]

9) Credentials

9.1) Credential_ID

----------
<a name="threats"/>
###Threats
####Credential Service-01
Threat: 
Threat Agent:
>

Attack Vectors:
>

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


  [1]:images/AddCredential.png
  [2]:images/UpdateCredential.png
  [3]:images/DeleteCredential.png
  [4]: Keystone_asset_library.md
