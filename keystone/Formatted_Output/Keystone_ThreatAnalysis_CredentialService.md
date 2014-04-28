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
 -  The path for policy.json file is rightly configured in the Keystone configuration file. In addition, it can be enforced.
   
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
SSL protected port, used to access the keystone server. External requests come and return through this port. Default 5000. If you plan to use SSL proxy, it could be different.

####Persistence layer (DB):
Token creation phase data is stored in DB, validation phase data is retrieved from DB.

----------
<a name="asset"/>
###Assets
Full assets list is documented in url

----------
<a name="threats"/>
###Threats

  [1]:/images/AddCredential.png
  [2]:/images/UpdateCredential.png
  [3]:/images/DeleteCredential.png
