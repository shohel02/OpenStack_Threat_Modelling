
Threat Modeling
=========================================
### Table of contents

[Definitions](#definition)

[Modeling Phases](#modeling)

[Related Assumptions for the Threat Modeling](#assumption)

[Identification of Threats](#identification)

-----
Threat modeling and analysis provides a complete view about the security of a system.  It is performed using a systematic and strategic way for identifying and enumerating threats to a system.

<a name="definition"/>
###Some Common Definition (RFC 2828)

####Vulnerability: 
> A flaw or weakness in a system's design, implementation, or      operation and management that could be exploited to violate the      system's security policy

####Threat:
> A potential for violation of security, which exists when there      is a circumstance, capability, action, or event that could breach      security and cause harm... a threat is a possible danger that might exploit a      vulnerability

####Attack: 
> An assault on system security that derives from an intelligent threat, to evade security services and violate the security policy of a system.”


<a name="modeling"/>
###Modeling Phases
We are currently following an iterative step, starting from the higher level – identifying major components, assets, and identification of threats from overall perspective.  In the second part, we are performing threat analysis for each of the earlier identified components..  Finally, we merge the analysis report to one.

```Questions: component breakdown or use case breakdown is better option ?```

**Figure 1: An iterative process for threat identification**
![enter image description here][1]


Currently, we follow simplified steps for modeling (Do not try to reinvent the wheel here – Adjusted the OWSAP steps)

**Figures 2: Simplified steps for threat modeling**

![enter image description here][2]


####Steps:

 1. Application/component overview (deployment, technologies, usage, assumptions)
 2. Security Objective of a Component/Sub components
 3. Decomposition (Data Flow Diagrams - DFD, Trust boundaries, entry/exit point, assets, security controls in place)
 4. Threats against assets (Derive known threats).  STRIDE can be used for threat identification and classification. 
    - Threat Quantification (Attacker model, DREAD, RISK Analysis)
    - Security Controls vs. Threats.
 5.	Vulnerability Exploitation in Code
 6.	Vulnerability exists but cannot  be exploitable due to attacker model
 7.	Security Guidance/ Security Test case developments

<a name="assumption"/>
###Related Assumptions for the Threat Modeling
#### Threat Agents:
- ID-U: Internet Attacker– Unauthorized
- ID-A: Internet Attacker – Authorized
- IN-I: Internal Attacker - Insider

####Threat Categorization

We can follow STRIDE model (to understand STRIDE model, look at OWSAP documents)

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
A set of known threats against which we perform threat analysis. The list is not exhaustive but a good starting point. Also, it is a time consuming tasks to check each and every threats and  attack vectors for each asset.  Details in  Threat_DB_Ref.xls

<a name="identification"/>
###Identification of Threats
In final analysis, we follow a simplified process as exemplified in figure 3. In this picture, top row elements are generated from the step 1 to 3 from figure 2. These are dynamic parameters. The bottom row is the result. The middle rows are static elements – defined in section 3.  

Based on this information, we identify threats  for each asset accessed or modified by the proxies (operations) in the data flows diagram.

**Figure 3: Threat Analysis - simplified**
![enter image description here][3]


####Logic to find related threats

```
# within each component

for each threat_consequences in STRIDE applied on an a asset:
    Derive a possible threat_A on the asset which breaks security properties
    for each operation in DataFlows:
           modified_Asset = operation(asset)
           attack_vectors = Find (threat tree) attack_vectors for  threat_A during the operation/environment
           ranked_Threat = Ranking(attack_vectors)

return ranked_Threat
```        


[1]: images/Threat_modeling_steps_high_level.png
[2]: images/Threat_Modeling_steps.png
[3]: images/Threat_model_combination.png

