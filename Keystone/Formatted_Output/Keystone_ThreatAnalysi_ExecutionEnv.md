
Keystone Threat Modeling : Execution Environment
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
   
####Application Description
   

####Additional Info
  

<a name="implementation"/>
###Implementation Overview
####Major Components
   

####Dependent components
  

####Description


<a name="assumption"/>
###System Assumptions (External Dependencies)

###Security Objective



<a name="dfd"/>
###Data Flow Diagrams 
####Name 

<a name="entry"/>
###Entry Points
####Name

----------
<a name="asset"/>
###Assets
Full assets list is documented in url



----------
####Execution Environment-001
Threats:
> Threats from execution domain separation

Threat Agent:
> Internal Attacker

Attack Vectors:
> An internal attacker can exploit the weakness of execution domain 
separation, perform interference and tampering of system data (e.g., 
CONF, Keys).

Security Weakness:
> 

Counter Measures:
> IT environment should maintain a security domain for the Keystone 
execution environment (e.g., Use SELinux or AppArmour to create a separate domain for Keystone Process)

Extra:
>  Probability:  variable (depends on system deployment)

>   Impact:  Medium

>   Related Info:

>   Comments:
     Link to Bug/mailing list or Tracking 

####Execution Env-002
Threats:
> Reliable Time Stamps

Threat Agent:
> Internal Attacker

Attack Vectors:
> Time stamps are important for crypto operations (e.g., during token generation) and log management. Lack of reliable time source breaks the
integrity of the system.

Security Weakness:
> Security hardening (This could be a threat based on deployment)

Counter Measures:
> 

Extra:
>  Probability:  variable (depends on system deployment)

>   Impact:  High

>   Related Info:

>   Comments:
     Link to Bug/mailing list or Tracking 

