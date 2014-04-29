Keystone Threat Analysis Index
====================================

[Keystone High Level Analysis][1]
[Keystone Asset Table][2]

####ID-1: Client

 - ID-1.1: Keystone client 
 - ID-1.2: Dashboard – related to user login sessions 
 - ID-1.3: Auth_Token

####ID-2: Keystone Application Server

 - ID-2.1: Token_auth middleware  
 - ID-2.2: Filters 
 - ID-2.3: Identity Service
 - ID-2.4: Auth Service 
 - ID-2.5: Token Service 
 - ID-2.6: Assignment service 
 - [ID-2.7: Catalog Service][3] 
 - ID-2.8: Policy Service 
 - [ID-2.9: Token provider][4] 
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


  [1]: Formatted_Output/Keystone_ThreatAnalysis_HighLevel.md
  [2]: Formatted_Output/Keystone_asset_library.md
  [3]: Formatted_Output/Keystone_ThreatAnalysis_CredentialService.md
  [4]: Formatted_Output/Keystone_ThreatAnalysis_TokenProvider_2.9.md