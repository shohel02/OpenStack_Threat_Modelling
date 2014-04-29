
Keystone Threat Modeling : Catalog Service
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
The Keystone Catalog Service falls under the Identity service. 
This document is in continuation with the Threat Modeling of Keystone Identity and Assignment Driver.

####Additional Info


<a name="implementation"/>
###Implementation Overview
####Major Components

Keystone Credential Controller.

Catalog Drivers (example SQL).

####Dependent components
Keystone Policy Engine.

####Description

<a name="assumption"/>

###System Assumptions (External Dependencies)
 -  The driver for Keystone Catalog Service used in this document is SQL.
 -  The path for policy.json file is rightly configured in the Keystone configuration file. In addition, it can be enforced.
   
###Security Objective
 - Provide authentic CRUD operation for Catalog Services.
 

<a name="dfd"/>
###Data Flow Diagrams 
####Create Endpoint
![Request Type: POST v3/endpoints
Operation: Adds an endpoint.
Normal Response Code: 201

API REQUEST:
{
   "endpoint":{
      "interface":"[admin|public|internal]",
      "name":"name",
      "region":"--optional--",
      "url":"...",
      "service_id":"--service-id--"
   }
}
][1]
####Update Endpoint
![Request Type:  PATCH  v3/endpoints/ {endpoint_id}
 Operation:  Updates a specified endpoint.
 Normal Response Code: 200
][2]

####Delete Endpoint
![Request Type: DELETE v3/endpoints/ {endpoints_id}
Operation: Deletes a specified endpoint.
Normal Response Code: 204

NOTE: This operation does not require a request body and does not return a response body
][3]

####Create Service
![Request Type: POST  v3/services
Operation: Adds a service.
Normal Response Code: 201

API REQUEST: 
{
    "type": "..."
}

][4]

####Update Service
![Request Type: PATCH v3/services/{service_id} 
Operation: Updates a specified service.
Normal Response Code: 200
][5]

####Delete Service
![Request Type: DELETE  v3/services/ {services_id}
Operation: Deletes a specified service.
Normal Response Code: 204

NOTE:
 
1. Deleting a service when endpoints exist should either (1) delete all associated endpoints or (2) fail until endpoints are deleted.

2. This operation does not require a request body and does not return a response body.

][6]

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

  [1]: images/CreateEndpoint.png
  [2]: images/UpdateEndpoint.png
  [3]: images/DeleteEndpoint.png
  [4]: images/CreateService.png
  [5]: images/UpdateService.png
  [6]: images/DeleteService.png
  