Keystone Threat Analysis Index File
====================================

[Keystone High Level Analysis][1]

[Keystone Asset Table][2]

####ID-1: Client

 - ID-1.1: Keystone client 
 - ID-1.2: Dashboard – related to user login sessions 
 - ID-1.3: [Auth_Token][3]

####ID-2: Keystone Application Server

#####Authentication: 
 - ID-2.4: [Auth Service][5] 
 - ID-2.5: [Token Service][6]
 - ID-2.5.1: [Token Controller V2][7]
 - ID-2.9: [Token provider][10] 
 - ID-2.11: [Trust][11]

#####Authorization & Provisioning:
 - ID-2.3: [Identity Service][4]
 - ID-2.6: [Assignment service][8] 
 - ID-2.7: [Credentials Service][9] 
 - ID-2.8: [Policy Service][14]

#####Others:
 - ID-2.1: Token_auth middleware (included in ID-2.2) 
 - ID-2.2: [Filters][12]
 - ID-2.10: Cache 

####ID-3: Persistence Storage
 - ID-3.1: SQL Backend 
 - ID-3.2: LDAP Backend

####ID-4: System
 - ID-4.1: Web server – WSGI server  
 - ID-4.2  [Execution environment][13] 
 - ID-4.3: Dependencies (PyPI, python packages) httplib2, requirements.txt
 - ID-4.4: Audit System

####ID-5: External
  - ID-5.1: External Auth system


  [1]: Keystone_ThreatAnalysis_HighLevel.md
  [2]: Keystone_asset_library.md
  [3]: Keystone_ThreatAnalysis_AuthToken.md
  [4]: Keystone_ThreatAnalysis_Identitya&Assignment.md
  [5]: Keystone_ThreatAnalysis_AuthService_2.4.md
  [6]: Keystone_ThreatAnalysis_TokenService_2.5.md
  [7]: Keystone_ThreatAnalysis_TokenControllerV2.0_2.5.md
  [8]: Keystone_ThreatAnalysis_Identitya&Assignment.md
  [9]: Keystone_ThreatAnalysis_CredentialService.md
  [10]: Keystone_ThreatAnalysis_TokenProvider_2.9.md
  [11]: Keystone_ThreatAnalysis_TrustService_2.11.md
  [12]: Keystone_ThreatAnalysis_APIv3Filter.md
  [13]: Keystone_ThreatAnalysi_ExecutionEnv.md
  [14]: Keystone_ThreatAnalysis_PolicyEngine.md 
