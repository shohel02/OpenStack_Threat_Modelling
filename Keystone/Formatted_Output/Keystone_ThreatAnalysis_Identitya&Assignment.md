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
Identity service provides data about Users (projects), Group, and as well as any associated metadata. In the basic case, all  data is managed by the service, allowing the service to manage CRUD associated with the data.

Starting from Havana, several of the entities previously managed via the Identity backend were divided into a new backend called Assignments.  

The Assignments  driver  manages:
- Projects (previously known as Tenants)
- Roles
- Role Assignments
- Domains

The Identity driver still manages:
- Users
- Groups (User assingment to Group)


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
 -  The path for policy.json file is configured in the Keystone configuration file.
 -  The default drivers for Identity and for Assignment service  is SQL.
   
###Security Objective
  
   Requester requirements:

 1. availability of CRUD operations on Users, Tenants, Groups, Roles and Domains
 2. non-repudiation of CRUD response from service (trust and accountability can replace this)
 3. audit log of request

  Service requirements:
  
 1. Maintain audit log of the requests/response
 2. Confidentiality of sensitive information (e.g., password never leaves without encryption)
 3. Maintain integrity of service behavior
 ..* Lenght check of users, tenant, domain, group, password
 ..* White listing of Input data

  Service requirements from Persistence layer:
 
 1. availability of service prvided by DB
 2. non-repudiation of request/response prodvided by DB (hard to proof, instead we can go for weak
 authenticity property and assume that DB will do its job (trust) and have audit log)
 


<a name="dfd"/>
###Data Flow Diagrams 

###Identity Operations:

####Create User

![Image Description][1]

(Password truncation is removed due to [bug][14] 1175904. Instead, it returns an error if password 
is long)

####Delete User
![Image Description][2]

- (deletion order delete_user, delete credentials, delete token)
- (delete user deletes user and assoicated role information, what about user association with group and group
assignement to role. Do we have stale data in such case.)
- delete token : deletes token. if deleted user is part of a trust (trustee or trustor), the process deletes 
  token associated. What about the trust relationship, shouldnot we delete all those. who is going to delete
  the trust?
- 





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
SSL protected port, used to access the keystone server. External requests are received and served through this port. Default port is 5000.

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

> Impact: Medium

> Related Info: Keystone Policy File:
https://wiki.openstack.org/wiki/OSSN/OSSN-0009
https://bugs.launchpad.net/keystone/+bug/1268751

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

###Issues:

Create User:

1. constrainted are placed in different places :
..* during authorization check role matching and domain id matching
..* inside DB: e.g., user_id (pk), name(not null), domain_id (not null), length check. DB constraints
are not tight e.g., domain_id is not bound to any fk relationship.
..* User controller do some check e.g., field length, not null, password encrypted. 

What is the best division of checks ?

2. user created without password ? (encryption happens to empty password- does openstack support this) 

Delete User:

1. what should be the order of deletion (credentials, token, user)?
2. Deletion means total deletion or disabling the user and its resources

Change Password:
1. 

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
  [14]: https://bugs.launchpad.net/keystone/+bug/1175904
