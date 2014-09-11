Threat Modelling
=========================================
### Table of contents

[Definitions](#definition)

[Modelling Phases](#modeling)

[Identification of Threats](#identification)

[Related Assumptions for Threat Modelling](#assumption)

-----
Threat modelling and analysis provides a comprehensive security view of a system. 
It is performed using a systematic and strategic way for identifying and 
enumerating threats to a system.

<a name="definition"/>
###Some Common Definition (RFC 2828)

####Vulnerability: 
> A flaw or weakness in a system's design, implementation, or operation and 
management that could be exploited to violate the system's security policy.

####Threat:
> A potential for violation of security, which exists when there is a 
circumstance, capability, action, or event that could breach security and cause
 harm... a threat is a possible danger that might exploit a vulnerability.

####Attack:
> An assault on system security that derives from an intelligent threat, to 
evade security services and violate the security policy of a system.

<a name="modeling"/>
###Modelling Phases
Currently, We are following two phase process. The first phase starts from a 
higher level:

- Selecting a target 
- Identifying major components
- Identification of threats from overall perspective
  
In the second phase, we perform threat analysis for each of the earlier 
identified components.

**Figure 1: Two phase process for threat identification**
![Figure 1][1]


The second phase of Threat modelling process follows a simplified version of 
Open Web Application Security Project([OWSAP][4]) recommended steps.

**Figures 2: Simplified steps for threat modeling**

![Figure 2][2]


Steps:

 1. Application/component overview (deployment, technologies, usage, assumptions)
 2. Security Objective of a Component/Sub components
 3. Decomposition (Data Flow Diagrams - DFD, Trust boundaries, entry/exit point,
    assets, security controls in place)
 4. Threats against assets - derived from known threats. STRIDE is used for 
    threat identification and classification.This also includes:
    - Threat Quantification (Attacker model, DREAD, Risk analysis)
    - Security Controls vs. Threats.
 5. Vulnerability exploitation in code
 6. Vulnerability exists but cannot be exploited due to the attacker model
 7. Security guidance/Security test case development.

<a name="identification"/>                                                      
###Identification of Threats                                                    
A simple process to identify threats is shown in Figure 3. Top row elements are 
inputs derived from the step 1 to 3 of threat modelling process while the bottom 
row elements represent result of the analysis.                                           
                                                                                
**Figure 3: Threat Modelling  - simplified**                                      
![Figure 3][3]                                              
                                                                                                                                                                
####Logic to identify threats                                               
                                                                                
```                                                                             
# within each component                                                         
                                                                                
for each threat_consequence in STRIDE applied on an asset:                   
    derive possible threat_A on the asset which breaks security properties    
    for each operation in DataFlows:                                            
           modified_Asset = operation(asset)                                    
           attack_vectors = Find (threat tree) attack_vectors for threat_A 
                            during the operation/environment
           ranked_Threat  = Ranking(attack_vectors)                              
                                                                                
return ranked_Threat                                                            
```                                                                             
                   
<a name="assumption"/>
###Common assumptions/guidelines for the threat modelling

#### Threat Agents:
- ID-U: Internet Attacker – Unauthorized
- ID-A: Internet Attacker – Authorized
- IN-I: Internal Attacker - Insider

####Threat Categorization
We follow STRIDE model for threat identification and classification
(to understand STRIDE model, please check OWSAP documents)

- SPOOFING
- TAMPERING
- REPUDIATION
- INFORMATION DISCLOSURE
- DENIAL OF SERVICE
- ELEVATION OF PRIVILEGE

####Likelihood of a Threat (Probability)
- Low
- Medium
- High

####Impact
- Low
- Medium
- High

####Threat Agent Capabilities (Not used in the current model)
- Level 0: Script Kiddies
- Level 1: Motivated individuals
- Level 2: Highly Capable Individuals
- Level 3: Serious Organized crime
- Level 4: Intelligence services

####Threat DB:
A set of known threats to assist identification of threat analysis (Not used 
in the current model)

####Template
A common [template][5] for threat analysis report. This provides
consistency across all reports.


[1]: images/Threat_modeling_steps_high_level.png
[2]: images/Threat_Modeling_steps.png
[3]: images/Threat_model_combination.png
[4]: https://www.owasp.org/index.php/Application_Threat_Modeling
[5]: Project_ThreatAnalysis_ComponentName_Number.md
