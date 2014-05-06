
Keystone Threat Modeling : Policy Engine
========================================
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
The Keystone policy service provides a role-based authorization engine.

Various components in the system require different actions to be performed based on a user's authorization.

The DFDs are based ‘Adding Users, Tenants and Roles with python-keystoneclient’ http://docs.openstack.org/grizzly/openstack-compute/admin/content/adding-users-tenants-and-roles-with-python-keystoneclient.html

*Only users with admin credentials can administer users, tenants and roles. You can configure the python-keystoneclient with admin credentials through either the authentication token, or the username and password method.

####Additional Info


<a name="implementation"/>
###Implementation Overview
####Major Components
Assert Admin

Policy Engine for Keystone 

Common Policy Engine. 

####Dependent components
Policy file (policy.json is only considered)  

Keystone configuration file.

####Description

<a name="assumption"/>

###System Assumptions (External Dependencies)
 -  Policy.json file is present.
 -  The path for policy.json file is rightly configured in the Keystone configuration file.
 -  The policy.json file can be loaded from the configuration file.
 -  The policy enforcer can be initialized.
    
###Security Objective
 - Authorize a user to perform operations based on his role and rule defined in policy.json file.
 

<a name="dfd"/>
###Data Flow Diagrams 

####Authorization in Keystone
![Image Description Here!!!][1]

####Policy Engine WorkFlow
![Image Description Here!!!][2]

####Keystone Policy Engine
![Image Description Here!!!][3]

####OpenStack Policy Engine
![Image Description Here !!!][4]

<a name="entry"/>
###Entry Points

####Public Port
SSL protected port, used to access the keystone server. External requests come and return through this port. Default 5000. If you plan to use SSL proxy, it could be different.



----------
<a name="asset"/>
###Assets
Full assets list is documented in url [Asset Library][5] 

9) Credentials

20) System


----------
<a name="threats"/>
###Threats

####Policy-01

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

####Policy-02
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

  [1]: images/DFD_Keystone_Policy_Authorization.png
  [2]: images/DFD_Keystone_Policy_PolicyEngineFlow.png
  [3]: images/DFD_Keystone_Policy_PolicyRule.png
  [4]: images/DFD_Keystone_Policy_CommonPolicyEngine.png
  [5]: Keystone_asset_library.md
 
  
  
