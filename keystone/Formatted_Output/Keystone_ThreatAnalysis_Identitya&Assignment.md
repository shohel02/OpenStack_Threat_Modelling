Keystone Threat Modeling : Identity and Assignment Service
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
The Identity service provides data about Users (projects), Group and as well as any associated metadata. In the basic case all this data is managed by the service, allowing the service to manage all the CRUD associated with the data.

Starting from Havana, several of the entities previously managed via the Identity backend were divided into a new backend called Assignments.  

The Assignments  driver  manages:
•	Projects (previously known as Tenants)
•	Roles
•	Role Assignments
•	Domains

The Identity driver still manages:
•	Users
•	Groups
•	Group Assignments


####Additional Info

<a name="implementation"/>
###Implementation Overview
####Major Components

Keystone Identity Controller.


Keystone Assignment Controller.

Identity and Assignment Drivers (example SQL, LDAP)

####Dependent components
Keystone Policy Engine.

####Description

<a name="assumption"/>

###System Assumptions (External Dependencies)
 -  Assignment and Identity drivers should be configured in the Keystone configuration file.
 -  The path for policy.json file is rightly configured in the Keystone configuration file. In addition, it can be enforced.
 -  The default drivers for Identity and for Assignment service  is SQL.
   
###Security Objective
 - Provide authentic CRUD operations on Users, Tenants, Groups, Roles and Domains.
 

<a name="dfd"/>
###Data Flow Diagrams 

###Identity Operations:

####Create User
![Image Description][1]

####Delete User
![Image Description][2]

####Change Password
![Image Description][3]

####Add User to Group
![Image Description][4]

####Remove Users From Group
![Image Description][5]

####Delete Group
![Image Description][6]

###Assignment Operations:

####Delete Domain
![Image Description][7]

####Delete Project
![Image Description][8]

####Create Role
![Image Description][9]

####Delete Role
![Image Description][10]

####Create Grant
![Image Description][11]

####Revoke Grant
![Image Description][12]

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
[Asset Library][13]

9) Credentials
20) System
----------
<a name="threats"/>
###Threats
####Assignment & Identity Service-01
Threat: According to the current Policy File, a domain admin can create a group and add user to the group.
If the Domain Admin becomes rogue and deletes the group then all tokens for a user beloging to the group are revoked.
Threat Agent:
>Domain Admin / Cloud Admin. 

Attack Vectors:
>Revoking all tokens for a user based on group membership is overkill, as we only would need to revoke tokens
that had role assignments via the group.  Calculating those assignments would have to be done by the 
assignment backend.


Security Weakness:
>

Vulnerable Component:
>Removing Users From Group.

Counter Measures:
> 

Extra:
> Probability: Low 

> Impact: High

> Related Info: Keystone Policy File: https://github.com/openstack/keystone/blob/master/etc/policy.v3cloudsample.json Referenced: 21 April 2014. 

> Comments: 

####Assignment & Identity Service-01
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

  [1]: images/CreateUser.png
  [2]: images/DeleteUser.png
  [3]: images/ChangePassword.png
  [4]: images/AddUserToGroup.png
  [5]: images/RemoveUserFromGroup.png
  [6]: images/DeleteGroup.png
  [7]: images/DeleteDomain.png
  [8]: images/DeleteProject.png
  [9]: images/CreateRole.png
  [10]: images/DeleteRole.png
  [11]: images/CreateGrant.png
  [12]: images/RevokeGrant.png
  [13]: Keystone_asset_library.md
  
