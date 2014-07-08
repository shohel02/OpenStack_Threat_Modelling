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
Keystone Havana Stable Release.
   
####Application Description
The Keystone policy service provides a role-based authorization engine. However, it is not strictly a role based authorization engine rather rules are matched for authorization.

####Additional Info

The DFDs are based on operations of ‘Adding Users, Tenants and Roles with python-keystoneclient’. http://docs.openstack.org/grizzly/openstack-compute/admin/content/adding-users-tenants-and-roles-with-python-keystoneclient.html

NOTE: Only users with admin credentials can administer users, tenants and roles. You can configure the python-keystoneclient with admin credentials through either the authentication token, or the username and password method.

Related Docs:
http://www.florentflament.com/blog/customizing-openstack-rbac-policies.html
http://docs.openstack.org/developer/keystone/configuration.html#keystone-api-protection-with-role-based-access-control-rbac


<a name="implementation"/>
###Implementation Overview
####Major Components
- Assert Admin (the context has is_admin set)
- Policy Engine for Keystone 
- Common Policy Engine. 

####Dependent components
- Policy file (policy.json is only considered)  
- Keystone configuration file.

####Description

<a name="assumption"/>

###System Assumptions (External Dependencies)
 -  Policy.json file is present.
 -  The policy enforcer is initialized.


###Security Objective
 - Authorize an authenticated user to perform requested action on a target resource based on token credentials, and rule defined in the policy.json file.
 

<a name="dfd"/>
###Data Flow Diagrams 

####Authorization in Keystone
![Image Description Here!!!][1]

1. PAP -- policy.json - contains rules containing roles/users allowed to perform actions on a target resource. E.g.
"identity:get_project":[["rule:admin_required"]]
2. PIP - User auth context (user_id,project_id,domain_id,role_id,group_id) from token. BuildAuthContextMiddleware fileter do the job for Ice House. For Havana, auth context is built up later on named as 'cred'
3. PDP - OpenStack common Policy engine
4. PEP - PEP exists at the relevant request routers. Acts based on response from PDP.


####Policy Engine WorkFlow - an example case (GET User)
![Image Description Here!!!][2]

####Keystone Policy Engine
![Image Description Here!!!][3]

####OpenStack Policy Engine
![Image Description Here !!!][4]

<a name="entry"/>
###Entry Points

####Controllers
Authorization resolving requests arrive from various service controllers (Which wants to protect resources).

####Token
To build auth credentials

####Policy_file
rule_set


----------
<a name="asset"/>
###Assets
Full assets list is documented in url [Asset Library][5] 

9) Credentials

20) System

creds, actions, target

rules

policy_file

----------
<a name="threats"/>
###Threats

####Policy-01

Threat: "admin"-ness not properly scoped (only for V2.0 API)
Threat Agent:
>Internal Attacker

Attack Vectors:
>No scoping of admin.

Security Weakness:
>User with an "admin" role on any tenant gets admin status throughout the system primarily because there is no demarcation between a scoped admin and a global admin.

Vulnerable Component:
>

Counter Measures:
> 

Extra:

> Probability: Low

> Impact: High

> Related Info: https://bugs.launchpad.net/keystone/+bug/968696

> Comments:

####Policy-02
Threat: Threats from Role management

Threat Agent:
>

Attack Vectors:
>Each OpenStack service has its own policy file, i.e., policy administration is decentralized. While
this approach distributes the load for authorization requests, this also increases possibility for misconfiguration (e.g, roles are not interpreted in the same way in different services). Another issue is scoping of role i.e., what pemissions (actions on resources) are allowed to each role and how each service interprest those permissions.

Security Weakness:
>

Vulnerable Component:
>Policy Administration

Counter Measures:
> 

Extra:
> Probability: Medium

> Impact: High

> Related Info:

> Comments:

####Issues:

Authorization process:

1. Its a rule engine not role. Nothing exists other than role-label
2. Only equlity matching, no other operator support.
2. Environment information is not taking into consideration.


OpenStack Policy engine:




  [1]: images/DFD_Keystone_Policy_Authorization.png
  [2]: images/DFD_Keystone_Policy_PolicyEngineFlow.png
  [3]: images/DFD_Keystone_Policy_PolicyRule.png
  [4]: images/DFD_Keystone_Policy_CommonPolicyEngine.png
  [5]: Keystone_asset_library.md
 
  
  
