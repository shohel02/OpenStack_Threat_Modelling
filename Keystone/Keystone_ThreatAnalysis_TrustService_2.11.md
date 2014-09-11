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
Trust provides project specific role delegation – i.e., a trustor delegates roles on a project to the trustee.  The trustee acquires the roles of the trustor for the specified period of time defined in the trust. Optionally, it allows impersonation – i.e., trustee takes the user_id of the trustor. Trust is consumed for creating a token scoped under trust_id. A trust_token cannot be used to create another trust_token.

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

 - Only trustor can create trust; trustor can create authentic trust (bounded by trustee, role, expiry time)
 - Only trustee can consume trust.


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

10) System admin 


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
####Trust-001
Threats:
>Repudiation threats from impersonation

Threat Agent:
>Internet Attacker - Authorized (Trustee)

Attack Vectors:
>If token impersonation is enabled and a trustor delegates trust to trustee, auditing who is responsible for a certian action can be difficult. (Audit log - is trust_id listed always along with user_id for any operation, if so at what log level ?)
 

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
>Repudiation of actions

Threat Agent:
>Internet Attacker - Autorized

Attack Vectors:
>Trust is not binded to a requestToken. A trustor can create a trust for a trustee 
even if the trustee did not request for such permission. 
This can happen due to negligence from the trustor or intentionally to implicate Trustee for an action.

Security Weakness:
> 

Counter Measures:
> Audit log

Extra:
>  Probability:

>   Impact:

>   Related Info:

>   Comments:
     Link to Bug/mailing list or Tracking 

####Trust-003
Threats:
>Threats arriving from unlimited/long expiry time in trust 

Threat Agent:
>Internet Attacker - Autorized

Attack Vectors:
> An attacker can set long enough / unlimited trust expiry time. Then 
use it to create a trust token.

Security Weakness:
> Expiry time is not limted by bound

Counter Measures:
> Expiry time bounded by CONF

Extra:
>  Probability:

>   Impact:

>   Related Info:

>   Comments:
     Link to Bug/mailing list or Tracking 


####Trust-004
Threats:
> Service(DB) exhaustion through unlimited number of trust creation 

Threat Agent:
> Interet Attacker- Authorized

Attack Vectors:
> An authorized user can create unlimited number of trust, this can lead to many 
record in trust db. 

Security Weakness:
> Should we have a limit on number of trust per user.

Counter Measures:
> 

Extra:
>  Probability:

>   Impact:

>   Related Info:

>   Comments:
     Link to Bug/mailing list or Tracking 


####Trust-005
Threats:
> Spoofing of trust by using the weakness of token

Threat Agent:
> Internet attacker - authorized

Attack Vectors:
> An internal service (possesor of token) can create a trust longer than
the value of the token ... the trust then can be used to get a trust token.
This allows an attacker enlarging the scope of the token. 

Security Weakness:
> Trust can be created from a weak creadential (trust creation from exising token is a  weakness)

Counter Measures:
> 

Extra:
>  Probability:

>   Impact:

>   Related Info:

>   Comments:
     Link to Bug/mailing list or Tracking 

####Trust-006
Threats:
> non-repudiation and unauthorized use of trust

Threat Agent:
> Internal attacker

Attack Vectors:
> Trust is not binded by cryptograpic signing allowing an internal attacker to change the
database and break the non-repudiation of trust

Security Weakness:
> trust should have cryptographic bind.

Counter Measures:
> 

Extra:
>  Probability:

>   Impact:

>   Related Info:

>   Comments:
     Link to Bug/mailing list or Tracking 


####Issues:

Create Trust:
1. Only trustor can create trust. Do we have auditng (log) who is creating the trust.
2. Can trust be created without providing impersonation ?
3. Trust is limited within single keystone server, how about trust federation ?


Delete Trust:

1. Who can delete trust - trustor, admin
2. What needs to be deleted when deleting trust: trust itself, token generated from trust.



  [1]: images/DFD_TRUST_ROUTER_V3.png
  [2]: images/DFD_TRUST_Create_Trust_v3.png
  [3]: images/DFD_TRUST_LIST_TRUST_V3.png
  [4]: images/DFD_TRUST_DELETE_TRUST.png
  [5]: images/DFD_TRUST_GET_TRUST.png
  [6]: Keystone_asset_library.md
