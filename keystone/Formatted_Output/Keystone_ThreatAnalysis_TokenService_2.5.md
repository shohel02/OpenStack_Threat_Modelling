Keystone Threat Modeling : Token Service
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
   Token Service is composed of two parts: Token Controller (providig access to token related V2 operation) and a generic token API (Used both by V2 and V3 API) includes the manager and the driver. The manager receives requests and fulfills the request with the help of specific driver. The driver is configurable using keystone.conf.  The driver in turn fulfills the operation by calling a persistent storage layer (SQL alchemy or other backend).
   
The V2 Token Controller routes and glues the requests from public router to the appropriate internal services. It also performs filtering on incoming and outgoing requests.  Authenticated users are issued bearer token.

####Additional Info
  

<a name="implementation"/>
###Implementation Overview
####Major Components
Keystone/token/controllers.py

Keystone/token/core
 
Keystone/token/backends/driver {sql/kvs/memchace}
####Dependent components
Dogpile.cache, SQLAlchemy, IDENTITY_API

####Description
Major process: 
Create_token

Validate_token

Delete_token

Revocation_list

<a name="assumption"/>
###System Assumptions (External Dependencies)

 - Only considered SQL backend
 - Parameters of the incoming request to TOKEN_API is trustworthy
 - The incoming request to TOKEN_API is trustworthy

###Security Objective

 - Fulfillment of intended API operation.


<a name="dfd"/>
###Data Flow Diagrams 
####Name 
image url
 


<a name="entry"/>
###Entry Points
####Name ID-01: Token Controller, Auth Controller
#####Description
Request from upper pipeline.
#####Accessible To
8) Keystone Process user

10)System admin 

####Name ID-03: Other Internal Services
#####Description
IDENTITY API, TOKEN PROVIDER API, TOKEN  API



----------
<a name="asset"/>
###Assets
Full assets list is documented in url



----------
####ComponentName-001
Threats:
> 

Threat Agent:
> 

Attack Vectors:
> 

Security Weakness:
> 

Counter Measures:
> 

Extra:
>  Probability:

>   Impact:

>   Related Info:

>   Comments:
     Link to Bug/mailing list or Tracking 

####ComponentName-002
Threats:
> 

Threat Agent:
> 

Attack Vectors:
> 

Security Weakness:
> 

Counter Measures:
> 

Extra:
> 

