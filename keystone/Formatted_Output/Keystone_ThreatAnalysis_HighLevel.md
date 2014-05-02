Keystone Threat Modeling - High Level
=========================================

### Table of Contents

[System Overview](#system)

[Implementation Overview](#implementation)

[System Assumptions](#assumption)

[Data Flow Diagrams](#dfd)

[Attacker and Actors](#attackers)

[Entry Points](#entry)

[Assets](#asset)

[Components](#components)

[Excluded List](#excluded)

[Threats](#threats)


<a name="system"/>
###System Overview
####Application version
   Keystone Havana Stable Release
   
####Application Description
   Keystone provides identity and access management for users of OpenStack Cloud.  For this, it provides a shim and most of the underlying components are pluggable. It provides authentication of users, can contain a repository of users (identities) and roles. It also performs account and entitlement management of users.  The entitlements are managed by assigning roles for users and enforcing access control on the target service (object).
   
   -	Username/user id identifies the user. A user is authenticated by a configured authentication mechanism. Before authentication, the account needs to be provisioned. Keystone also performs account provisioning e.g., users, tenant, domain, role, and their assignments. External authentication mechanisms e.g., Apache HTTP authentication can be integrated with keystone.
-	Authorization (entitlement) is performed for each request based on assigned role within the token for a user and policy file defined in the system. The entitlement decision is decentralized i.e., made by the respective service.
-	Each successful and unsuccessful request is logged.
-	User and system data is protected by access control and sensitive data (only password) is protected by one way hashing.
-	Keystone provides catalog information for other services.

   
####Additional Info
  

<a name="implementation"/>
###Implementation Overview
####Major Components
**Pipe line middlewares:**  url_nomalizer, token_auth, admin_token_auth, xml_body, Json_body, ec2_extension,  S3_extension,  crud_extension, admin_service, public_service (check the middleware from paste.ini for specific Pipe).  Currently this document focuses on [pipeline:api_v3], the keystone v3 API pipeline. The document also include some parts of [pipeline:public_api] – keystone v2 API pipeline.

**Services/internal components:** Identity, Assignment, Catalog, policy, Token, Token Provider, Trust

**Drivers/persistence storage:**  SQL, FILE, KVA, LDAP


####Dependent components
**Webservers:**  WSGI (or any web server on top WSGI modules can run) 

**Operating system and libraries:**  oslo.config, dependencies. 

**Crypto:**  OpenSSL library, hashlib, passlib, UUID generator (Crypto library versions are considered)
Related info covered in:  
https://wiki.openstack.org/wiki/Security/Juno/Keystone#Notable_changes_since_Icehouse
####Description

-	Keystone consists of a combination of services: identity, assignment, catalog, policy, token and so on. Each of the service has specific drivers, which can be plugged using configuration profile.
-	Keystone performs access control: authentication and authorization (entitlement) of users. The authentication process can be performed using three mechanisms: username/password, token, and External authentication (including external authentication middleware or an external authentication mechanism which set REMOTE_AUTH variable). A successful authentication issues a token to the requester.
-	The authorization process consists of assignment of roles, policy definition file specific for specific OpenStack service, entitlement decision engine and enforcement points. 
-	Keystone provides identity provisioning service e.g., users, tenants, groups, roles, domains 
-   As an addition, Keystone provides catalog services   (description of     services and endpoints) for each user.

<a name="assumption"/>
###System Assumptions (External Dependencies)
####Application

- Keystone running in WSGI server which is accessible via public port 5000 and admin port 35357
- The document will cover both V2.0 (limited only for Token service) and V3.0 API.
- Keystone.conf, unless stated otherwise, the default values of the configuration file is used (keystone with devstack setting)
- PKI token (default token provider)
- SSL endpoint (Keystone server endpoint. We assumed keystone server to client transport channel is SSL protected.     OpenStack Security Guide and Keystone doc define steps for SSL endpoints)
- The communication channel between client and keystone service endpoint is SSL protected (from assumption 4).

####Persistence 
- The database server is MySQL and related driver is SQLAlchemy.
- Default config DB from keystone.conf and mysql.conf

####Platform
- Operating systems, webserver, and physical machines are hardened
- The system admin follows security best practice
- Keystone server is running in Ubuntu 12.04
- Dependencies (keystone dependent libraries) in requirements.txt  are trustworthy

###Security Objective

 -  An IdAM must provide
-	Identification (authentication) and entitlements of users
-	Entitlement decision (authorization) for user-initiated request
-	Non-repudiable auditing for each request
-	Protection of system and user data / state information.
-   In addition, for keystone case: 
    -    Integrity of catalog information 

<a name="dfd"/>
###Data Flow Diagrams 
####Keystone Internal Components and Threat Agents
![enter image description here][1]


<a name="attackers"/>


----------
###Attackers and Actors
Interfaces through which actors/attackers can request for assets/resources or interfaces through which the system returns asset information to the requester. 

####Name: IA-U: Internet Attacker– Unauthorized
#####Actors
ID-1. Anonymous
#####Details

####Name: IA-A: Internet Attacker– authorized
#####Actors
ID-2. Project User

ID-3. Owner

ID-4. Project Admin

ID-5. Domain Admin

ID-6. Cloud Admin

ID-7. Admin (V2 case)
#####Details

####Name: IA-A: Internal Attacker
#####Actors
ID-8.  Keystone System user

ID-9.  Other System user

ID-10. System Admin

ID-11. DB Admin

ID-12. Keystone DB user

ID-13. External Identity provider user

ID-14. service user

#####Details

----------
<a name="entry"/>
###Entry Points
####Name: ID-01: Public Port
#####Description
SSL protected port, used to access the keystone server. External requests come and return through this port. Default 5000. If you plan to use SSL proxy, it could be different
#####Accessible To
All, however only authenticated user can get in.

####Name: ID-02: Admin port
#####Description
SSL protected port, used to access the keystone server. External requests come and return through this port. Default 35357. If you plan to use SSL proxy, it could be different. Used only in V2 API
#####Accessible To
All, however, only admin role user can get in (V2 case)

####Name: ID-03: External (optional)
#####Description
Protected, used to authenticate users (user data) from external system
#####Accessible To
(8) Keystone System user
(10) System Admin (Does system admin needs this ?)
can access remote system to verify user data. Not the other way around.

####Name: ID-04 Interface towards persistence layer
#####Description
Keystone server communicates with database backend . We only consider MySQL server.
#####Accessible To
(8) Keystone System user
access DB (through some DB user). Not the other way around. (10) Does System Admin needs this access?

####Name: ID-05 Cache interface
#####Description
Keystone uses dogpile cache layer which stores data in one of the cache backend.
#####Accessible To
(8) Keystone System user
access cache server. Not the other way around. (10) Does System Admin needs this access? 

####Name: ID-06 Configuration and key materials
#####Description
Keystone uses configuration parameter during initialization. It also uses key materials for signing and verifying the PKI key
#####Accessible To
(8) Keystone System user
access. (10) Does System Admin 


----------
<a name="asset"/>
###Assets
Asset library URL.
[enter link description here][2]

----------
<a name="controls"/>
##Existing Security Controls


----------
<a name="components"/>
###Components
Components layering two trust boundaries and performing critical operations are interesting for the analysis

####ID-1: Client

 - ID-1.1: Keystone client 
 - ID-1.2: Dashboard – related to user login
   sessions 
 - ID-1.3: Auth_Token

####ID-2: Keystone Application Server

 - ID-2.1: Token_auth middleware  
 - ID-2.2: Filters 
 - ID-2.3: Identity Service
 - ID-2.4: Auth Service 
 - ID-2.5: Token Service 
 - ID-2.6: Assignment service 
 - ID-2.7: Catalog Service 
 - ID-2.8: Policy Service 
 - ID-2.9: Token provider 
 - ID-2.10: Cache 
 - ID-2.11: Trust

####ID-3: Persistence Storage

 - ID-3.1: SQL Backend 
 - ID-3.2: LDAP Backend

####ID-4: System

 - ID-4.1: Web server – WSGI server 
 - ID-4.2: SSL server 
 - ID-4.3 Operating system 
 - ID-4.4: Dependencies ( PyPI, python packages) httplib2, requirements.txt

####Name: ID-5: External
  - ID-5.1: External Auth system
  
(Note: The naming of asset and component is sometimes same. There are distinction between asset and component: an asset is something which has value and has a state. Component or proxies on the other hand manipulates /performs operation on top of asset. E.g., Trust is an asset – meaning it defines a relational state while ‘Trust component’ is the process to generate Trust asset)

----------
<a name="excluded"/>
###Excluded List
Excluded from current Analysis

----------


<a name="threats"/>
###Threats
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


  [1]: images/DFD_Attacker_Keystone_level_1.png
  [2]: Keystone_asset_library.md
