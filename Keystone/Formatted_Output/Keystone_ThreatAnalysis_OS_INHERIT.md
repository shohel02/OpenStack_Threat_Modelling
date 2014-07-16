Keystone Threat Modeling : OS-INHERIT
=========================================
### Table of contents

[System Overview](#system)
Provide an ability for projects to inherit roles from their owning domain.

A role assigned to a domain to optionally be able to be inherited by the owned projects. That way a cloud provider could assign, for example, a maintenance-role to an admin user (or group) on each domain they create, which would be automatically be included in a token they scoped to any project for issuing maintenance commands.

https://blueprints.launchpad.net/keystone/+spec/inherited-domain-roles

[Implementation Overview](#implementation)

https://github.com/openstack/identity-api/blob/master/v3/src/markdown/identity-api-v3-os-inherit-ext.md



[System Assumptions](#assumption)

[Data Flow Diagrams](#dfd)

[Entry Points](#entry)

[Assets](#asset)

[Threats](#threats)


<a name="system"/>
###System Overview
####Application version
   dd
   
####Application Description
   

####Additional Info
  

<a name="implementation"/>
###Implementation Overview
####Major Components
   

####Dependent components
  

####Description



<a name="assumption"/>
###System Assumptions (External Dependencies)

 - item 1

###Security Objective

 - item 1


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

