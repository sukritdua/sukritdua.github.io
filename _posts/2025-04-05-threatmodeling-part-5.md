---
title: "Threat Modeling - Part 5"
date: 2025-04-10 00:00:00 +/-TTTT
categories: [Series, Threat Modeling]
tags: [threatmodeling, threatdragon, owasp, stride]     # TAG names should always be lowercase
---

## What will we learn?

* Microsoft Threat Modeling Tool Basics
    
* System Threat Modeling
    
* Threat Generation
    
* It’s a wrap on this series!
    

## Threat Modeling - Microsoft TMMT

Download Link - [https://aka.ms/threatmodelingtool](https://aka.ms/threatmodelingtool)

Once you have it installed and ready to go let’s begin with a simple web application.

### Scenario

Here is our basic web application scenario drawn out in Lucidchart (another tool that can be used for threat modeling).

<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1739986794223/8a44520f-f5bf-4178-bed3-2178fdb38a4c.png" width="850" height = "225" alt="tmmt1.png">

### Instructions

```plaintext
1. Start by opening up the newly installed Microsoft Threat Modeling Tool
2. On the Welcome page, click 'Create A Model'
3. Using the scenario above, use the Stencil bar on the right side to grab the different elements
    1. Browser 
    2. HTTPS communication
    3. HTTP communication
    4. Web Application
    5. SQL Database
```

<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1739986998473/0427a5fa-2ebe-4813-8622-8810f946e6fe.png" width="650" height = "225" alt="tmmt2.png">

## Threat Generation

Now for the exciting part…we have the simple web application with interactions diagrammed out. To generate threats, simple click ‘View’ &gt; ‘Analysis View’

The Threat list should populate in the bottom pane and voila you can see the STRIDE categories and the threats mapped to it. It can be exported as a CSV and/or we can generate an HTML report.

<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1739987151889/d5ba7bd1-fe9d-4ce3-8e26-d5f7175dda15.png" width="750" height = "425" alt="tmmt3.png">

*Note: This is a very generic threat generation sample model to show case what the interface looks like and how you can be on your way to threat modeling. The threats generated are a good starting point and should not be considered gospel.*

Clicking on the individual threat opens up the threat properties as shown. We can choose priority, Status and make other changes.

<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1739987406364/88a44ea1-827c-4c80-881a-11fa8db375ab.png" width="950" height = "325" alt="tmmt4.png">


The next step here would be to add context, custom attributes, custom threats (within templates), trust zones/boundaries, and enrich this diagram further.

## Report Generation

`‘Report’ > Generate Report`

<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1739987490527/4edd7467-f812-41f4-a096-2e10a47d94ac.png" width="550" height = "525" alt="tmmt5.png">

We can drill down to individual data flows to understand the generated threats and their corresponding STRIDE categories as is visible below.

<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1739987584552/d4fcf60a-214b-412e-8f88-93385dd854ca.png" width="750" height = "425" alt="tmmt6.png">

## Conclusion

This series was a very watered down and high level entry into Threat modeling. I would recommend trying out Threat modeling with this tool and reading up the following:

1. [https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool)
    
2. [https://shostack.org/books/threat-modeling-book](https://shostack.org/books/threat-modeling-book)