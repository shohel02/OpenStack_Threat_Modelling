Keystone Threat Analysis Index
====================================

[Keystone High Level Analysis][1]

[Keystone Asset Table][2]

####ID-1: Client

 - ID-1.1: Keystone client 
 - ID-1.2: Dashboard – related to user login sessions 
 - ID-1.3: [Auth_Token][3]

####ID-2: Keystone Application Server

 - ID-2.1: Token_auth middleware  
 - ID-2.2: [Filters][12]
 - ID-2.3: [Identity Service][4]
 - ID-2.4: [Auth Service][5] 
 - ID-2.5: [Token Service][6]
 - ID-2.5.1: [Token Controller V2][7]
 - ID-2.6: [Assignment service][8] 
 - ID-2.7: [Catalog Service][9] 
 - ID-2.8: [Policy Service][14] 
 - ID-2.9: [Token provider][10] 
 - ID-2.10: Cache 
 - ID-2.11: [Trust][11]
 -

####ID-3: Persistence Storage

 - ID-3.1: SQL Backend 
 - ID-3.2: LDAP Backend

####ID-4: System

 - ID-4.1: Web server – WSGI server 
 - ID-4.2: SSL server 
 - ID-4.3  [Execution environment][13] 
 - ID-4.4: Dependencies ( PyPI, python packages) httplib2, requirements.txt
 - ID-4.5: Audit System

####Name: ID-5: External
  - ID-5.1: External Auth system


  [1]: Formatted_Output/Keystone_ThreatAnalysis_HighLevel.md
  [2]: Formatted_Output/Keystone_asset_library.md
  [3]: Formatted_Output/Keystone_ThreatAnalysis_AuthToken.md
  [4]: Formatted_Output/Keystone_ThreatAnalysis_Identitya&Assignment.md
  [5]: Formatted_Output/Keystone_ThreatAnalysis_AuthService_2.4.md
  [6]: Formatted_Output/Keystone_ThreatAnalysis_TokenService_2.5.md
  [7]: Formatted_Output/Keystone_ThreatAnalysis_TokenControllerV2.0_2.5.md
  [8]: Formatted_Output/Keystone_ThreatAnalysis_Identitya&Assignment.md
  [9]: Formatted_Output/Keystone_ThreatAnalysis_CredentialService.md
  [10]: Formatted_Output/Keystone_ThreatAnalysis_TokenProvider_2.9.md
  [11]: Formatted_Output/Keystone_ThreatAnalysis_TrustService_2.11.md
  [12]: Formatted_Output/Keystone_ThreatAnalysis_APIv3Filter.md
  [13]: Formatted_Output/Keystone_ThreatAnalysi_ExecutionEnv.md
  [14]: Formatted_Output/Keystone_ThreatAnalysis_PolicyEngine.md 
