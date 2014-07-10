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
The Keystone Credential Service provides CRUD operation on credentials.
Why: to store key pair in keystone . 
https://blueprints.launchpad.net/nova/+spec/credentials-from-keystone
any other reason.

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
 - Provide authentic CRUD operations for Credential Services 
 

<a name="dfd"/>
###Data Flow Diagrams 
####Create Credentials
![enter image description here][1]

- note: based on Ice-house release
- 

####UpdateCredentials
![enter image description here][2]
####DeleteCredentials
 ![enter image description here][3]

<a name="entry"/>
###Entry Points

####router
Request arriving from router /processing pipeline.

####Persistence layer (DB):
Credential storage and retrieval.

----------
<a name="asset"/>
###Assets
Full assets list is documented in url
[Asset Library][4]

- Credentials
- Credential_ID
- trust_id

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
