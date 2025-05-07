---
title: "Threat Modeling - Part 2"
date: 2025-02-10 00:00:00 +/-TTTT
categories: [Series, Threat Modeling]
tags: [threatmodeling, threatdragon, owasp]     # TAG names should always be lowercase
---

# What will we learn?

* Trust Boundaries
    
* Trust Zones
    
* Scoping
    
* What to ask yourself as you begin threat modeling
    

# Questions to ask yourself when you start (4Qs by Adam Shostack)

* What are you working on? Scope
    
* What can go wrong? Heart of the threat model
    
* What will you do about it? Think about mitigating controls, prioritization etc.
    
* Did you do an acceptable job? Peer approval, controls etc.
    

Let’s jump right in!

## What are you working on?

Say we have an **application** for our threat modeling exercise, here are the items you need or may have to develop as you go through (not everything is needed but helps with context)

* Process Flow Diagrams
    
* Data Flow Diagrams
    
* Network Diagrams
    
* Vulnerability Assessment Results
    
* OSINT Results
    

The idea is to get started with the bare minimum and build from there as this is an iterative process.

## Trust Boundaries

As the name suggests, any entity that is external to your system warrants a trust boundary. Based on the scope, there should be boundaries talking about the same level of trust and there should be additional security controls for every change in trust level. For example, a user of your web application is an external entity and should not be trusted.

* Authorization → Increased / Decreased/ Separate Privilege level
    
* Ownership → Do I own the system? If not, then that’s a trust level change
    
* Communication → Are all my systems communicating with each other are in the same level of trust?
    

The cons of boundaries are they are vague with evolving systems architecture like cloud/low/zero trust environments. It is a slow process. Instead Trust Zones were created.

## Trust Zones

Create Zones instead of boundaries by marking 0 (low trust) - 9 (high trust).

* Any entities out of your control will be 0 trust like an end user on your application.
    
* Boundary entities - Front End / API gateway etc. are marked as 1 → Validation and Sanitization necessary
    
* Whenever data is being written to disk like an SSO Store/ DB - 9 (could be 6-9)
    
* Idea is to think about specific functionalities and rank them accordingly like - Login page is publicly accessible would be a 2 but a provisioning service on the back end could be a 7/8.
    

Anywhere the trust zones change is where the potential risks will exist.

# Tools:

You can pretty much use a pen and paper/whiteboard or MS Paint for threat modeling, however there are some tools out there that can help make the process easier especially for documentation and threat generation.

* OWASP Threat Dragon ([https://github.com/OWASP/threat-dragon/releases](https://github.com/OWASP/threat-dragon/releases))
    
* Microsoft Threat Modeling Tool ([https://aka.ms/threatmodelingtool](https://aka.ms/threatmodelingtool)) - **Recommended**
    

## Sample Threat model in OWASP Threat Dragon

<img src="https://cdn.hashnode.com/res/hashnode/image/upload/v1738615773689/020cb938-509c-4a7e-97a2-0e7f4df34e12.png" width="550" height="400" alt="threatdragon.png">

## Sample Threat model in Microsoft Threat modeling tool

<img src="https://camo.githubusercontent.com/4ca11d257d54a450821970964d19b8c29b48abeac2ac7350004e00d3d232ccf2/68747470733a2f2f692e696d6775722e636f6d2f4d366f37774a542e706e67" width="950" height="450" alt="mttm.png">

Join me on the next post where I walk through a simple Threat Model in terms of creating boundaries and then identifying threats using STRIDE.

