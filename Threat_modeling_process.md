Threat modeling
=========================================
### Table of contents

[Definitions](#definition)

[Modeling phases](#modeling)

[Identification of threats](#identification)

[Related assumptions for threat modeling](#assumption)

-----
Threat modeling and analysis attempts to provide a comprehensive security view
of a system. It is performed using a systematic and strategic way for
identifying and enumerating threats to a system.

<a name="definition"/>
###Some common definition (RFC 2828)

####Vulnerability:
> A flaw or weakness in a system's design, implementation, or operation and
management that could be exploited to violate the system's security policy.

####Threat:
> A potential for violation of security, which exists when there is a
circumstance, capability, action, or event that could breach security and cause
harm...a threat is a possible danger that might exploit a vulnerability.

####Attack:
> An assault on system security that derives from a threat, to evade security
services and violate the security policy of a system.

<a name="modeling"/>
###Modeling phases
Currently, we are following a two phase process. The first phase starts from
higher level:

- Select a target project
- Identify major components
- Identify threats from overall perspective

In the second phase, we perform threat analysis for components identified during
the first phase.

**Figure 1: Two phase process for threat identification**
![Figure 1][1]


The second phase of threat modeling process follows a simplified version of
recommended steps defined by the Open Web Application Security Project([OWSAP][4]).

**Figures 2: Simplified steps for threat modeling**

![Figure 2][2]


Steps:

 1. Application/component overview (application version, deployment model,
    technologies used, assumptions).
 2. Security objectives of a component.
 3. Decomposition (Data Flow Diagrams - DFD, Trust boundaries, entry/exit points,
    assets, existing security controls).
 4. Threat identification against assets. We use STRIDE[6] model for
    identification and classification of threats.
 5. Threat quantification (using either Attacker model, DREAD[7], Risk analysis)
 6. Security guidance/security test case development.

<a name="identification"/>
###Identification of threats
A simple process to identify threats is shown in figure 3. Top row elements are
inputs derived from the step 1 to 3 of threat modeling steps, while the bottom
row elements are result from the analysis.

![Figure 3][3]


####A simple logic to identify threats
```
# within each component

for each threat_consequence in STRIDE applied on an asset:
    derive possible threat_A on the asset which violates security properties
    find attack_vectors (threat tree) for threat_A in the data flows of the component
    ranked_threat  = Ranking (attack_vectors)

return ranked_threat
```

<a name="assumption"/>
###Common assumptions/guidelines for threat modeling

#### Threat Agents:
- ID-U: Internet Attacker – Unauthorized
- ID-A: Internet Attacker – Authorized
- IN-I: Internal Attacker - Insider

####Threat categorization
We follow STRIDE model for threat identification and classification
(please check the link[6] to understand STRIDE model)

- SPOOFING
- TAMPERING
- REPUDIATION
- INFORMATION DISCLOSURE
- DENIAL OF SERVICE
- ELEVATION OF PRIVILEGE

####Likelihood of a Threat (probability)
- Low
- Medium
- High

####Impact
- Low
- Medium
- High

####Threat agent capabilities (Not used in the current model)
- Level 0: Script kiddies
- Level 1: Motivated individuals
- Level 2: Highly capable individuals
- Level 3: Serious organized crime
- Level 4: Intelligence services

####Threat DB:
A set of known threats to assist identification of threat analysis (not used
in the current model)

####Template
A common [template][5] for threat analysis report. This provides
consistency across all reports.


[1]: images/Threat_modeling_steps_high_level.png
[2]: images/Threat_Modeling_steps.png
[3]: images/Threat_model_combination.png
[4]: https://www.owasp.org/index.php/Application_Threat_Modeling
[5]: Project_ThreatAnalysis_ComponentName_Number.md
[6]: http://msdn.microsoft.com/en-us/library/ee823878(v=cs.20).aspx
[7]: https://www.owasp.org/index.php/Threat_Risk_Modeling#DREAD
