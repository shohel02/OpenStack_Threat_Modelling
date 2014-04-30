Keystone Threat Modeling : Trust
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
Trust provides project specific role delegation – i.e., a trustor delegates some roles on a project to the trustee.  The trustee acquires the roles of the trustor for the specified amount of time defined in trust. Optionally, it allows impersonation – i.e., trustee takes the user_id of the trustor. Trust is consumed by creating a token scoped under trust_id. A trust_token cannot be used to create another trust_token.

https://github.com/openstack/identity-api/blob/master/openstack-identity-api/v3/src/markdown/identity-api-v3-os-trust-ext.md
####Additional Info
  

<a name="implementation"/>
###Implementation Overview
####Major Components
Trust/routers – routing of HTTP call

Trust/controller – associated controller for a HTTP call

Trust/core – stub/logic for backend

Trust/backends/sql -  backend  implementation

Trust/backends/kvs - backend


####Dependent components
Token API

Identity API
####Description



<a name="assumption"/>
###System Assumptions (External Dependencies)
 - Trust for V3 API is only considered

###Security Objective

 - 

<a name="dfd"/>
###Data Flow Diagrams 
####Trust Router
![enter image description here][1]
####Create Trust
![enter image description here][2]
####List Trust
![enter image description here][3]
####Delete Trust
![enter image description here][4]
####Get Trust
![enter image description here][5]

<a name="entry"/>
###Entry Points

####Name ID-01: upstream_router
#####Description
Request from upper pipeline.
#####Accessible To
8) Keystone Process user

10)System admin 



----------
<a name="asset"/>
###Assets
Full assets list is documented in [url][6]

1)   User ( user_id)
7)   Trust (trust_id, Trustor_id, Trustee_id, project)
6.1) User_project (role grant)
4)   Role
35)  Context



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

####Trust-001
Threats:
>  Repudiation threats from impersonation

Threat Agent:
>  Trustee

Attack Vectors:
> If token impersonation is enabled , ability to track the events persformed by the impersonated trustee. 

A trustee delegates the trust to many other users with impersonation enabled, allowing many users in the system to use the same user id. 

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

####Trust-002
Threats:
>  Repudiation of actions

Threat Agent:
> trustor

Attack Vectors:
> Trust is not binded to requestToken. A trustor can create a trust for a trustee 
even if trust is not requested. This can happen due to negligence from the trustor
or intentionally to implicate Trustee for an action.

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


  [1]: images/DFD_TRUST_ROUTER_V3.png
  [2]: images/DFD_TRUST_Create_Trust_v3.png
  [3]: images/DFD_TRUST_LIST_TRUST_V3.png
  [4]: images/DFD_TRUST_DELETE_TRUST.png
  [5]: images/DFD_TRUST_GET_TRUST.png
  [6]: Keystone_asset_library.md