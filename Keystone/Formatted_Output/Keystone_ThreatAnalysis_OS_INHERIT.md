Keystone Threat Modeling : OS-INHERIT
=========================================
### Table of contents

[System Overview](#system)


[Implementation Overview](#implementation)



[System Assumptions](#assumption)


[Data Flow Diagrams](#dfd)

[Security Objective](#objective)

[Entry Points](#entry)


[Assets](#asset)


[Threats](#threats)


<a name="system"/>
###System Overview
Provide an ability for projects to inherit roles from their owning domain.

A role assigned to a domain to optionally be able to be inherited by the owned projects. That way a cloud provider could assign, for example, a maintenance-role to an admin user (or group) on each domain they create, which would be automatically be included in a token they scoped to any project for issuing maintenance commands.

https://blueprints.launchpad.net/keystone/+spec/inherited-domain-roles

####Application version
   
####Application Description
   
####Additional Info
  

<a name="implementation"/>
###Implementation Overview
https://github.com/openstack/identity-api/blob/master/v3/src/markdown/identity-api-v3-os-inherit-ext.md

https://etherpad.openstack.org/p/inhertited-domain-roles

API overview:

Assign role to user on projects owned by a domain:

PUT /OS-INHERIT/domains/{domain_id}/users/{user_id}/roles/{role_id}/inherited_to_projects

Assign role to group on projects owned by a domain:

PUT /OS-INHERIT/domains/{domain_id}/groups/{group_id}/roles/{role_id}/inherited_to_projects

List user's inherited project roles on a domain:

GET /OS-INHERIT/domains/{domain_id}/users/{user_id}/roles/inherited_to_projects

List group's inherited project roles on domain:

GET /OS-INHERIT/domains/{domain_id}/groups/{group_id}/roles/inherited_to_projects

####Major Components
   

####Dependent components
  

####Description



<a name="assumption"/>
###System Assumptions (External Dependencies)
Config file:
config.CONF.os_inherit.enabled  


###Security Objective
- authentic assignment of domain role to the project user 
- authentic assignment of domain role to the project group


<a name="dfd"/>
###Data Flow Diagrams 
####Name 
image url
 


<a name="entry"/>
###Entry Points
####Name
  data



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

