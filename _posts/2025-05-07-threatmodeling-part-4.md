---
title: "Threat Modeling - Part 4"
date: 2025-03-15 00:00:00 +/-TTTT
categories: [Series, Threat Modeling]
tags: [threatmodeling, threatdragon, owasp, stride]     # TAG names should always be lowercase
---

# What will we learn?

* System Threat Modeling approaches
    
* Unconventional Threat Modeling Techniques
    
* Contrasting Threat Modeling approaches
    
* Success Criteria
    
* Strategies for effective Threat Modeling
    
* System Threat modeling with STRIDE
    

# System Threat Modeling Process

1. Define Objectives and Security Requirements
    
    1. Could be compliance driven like FISMA, PCI etc.
        
    2. Could be management security objectives
        
    3. Contractual requirements
        
    4. Company policies
        
2. Define Scope → Technical Scope, Network, Components, apps and more
    
    1. Larger your scope the more time consuming and the more diluted your Threat Model is
        
    2. Bound your scope
        
    3. Threat models are iterative and need improvements over time so do not have an extensive scope
        
3. Decompose/Simplify Application
    
    1. Data Flow Diagrams
        
    2. Network Diagrams
        
    3. Trust Boundaries and Zones.
        
4. Threat Assessment → Create likely Threat Scenarios
    
5. Next Steps → Mitigation/Risk, Attack Models, Vuln analysis and more.
    

# Success Factors

* Threat Model and then validate the Threat model with a pentest/VA/Red Team
    
* Cross functional team for collaboration on the Threat Model
    
* Time box these activities for optimal results
    
* Get it done, don’t wait for perfect conditions (people, process, etc)
    

# General guidelines for a system Threat Model

We are assuming we have a network diagram or a DFD with Trust Zones/Boundaries to begin our modeling process.

1. Map out the **E**scalation of Privileges Threats first
    
2. Map out the **S**poofing Threats
    
3. Map out the **T**ampering Threats
    
4. Map out the **R**epudiation Threats
    
5. Map out the **I**nformation Disclosure Threats
    
6. Map out the **D**enial of Service Threats
    

This is a good way to go about approaching your first pass at the Threat Modeling process.

# Threat Modeling Strategies

* Brainstorming
    
    * Well D’uh
        
* Pre-Mortem Analysis
    
    * “Assumption of failure”
        
    * Think of horrible situations and build from there
        
    * Engages all folks including management/non-tech folks
        
* Attack Trees
    
    * Attack trees are conceptual diagrams that show the variety of ways in which something can go wrong, and the reason why they might go wrong. A sample attack tree:
        
        <img src="https://www.ncsc.gov.uk/static-assets/images/guidance/FC%20root%20node.svg" width="700" height="600" alt="nhs.png">

        *Source:[NCSC](https://www.ncsc.gov.uk)*
        
* Movie style plotting
    
* [Elevation of Privilege Card game](https://shostack.org/games/elevation-of-privilege)
    
    * Card game to help increase awareness and engagement in Threat Modeling activities
        
    * Sort of outdated now as it was created with Desktop applications in mind and not modern cloud infrastructure

<br>

See you in the next one!