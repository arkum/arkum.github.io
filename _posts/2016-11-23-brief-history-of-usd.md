---
layout: post
title: Brief History of USD
subtitle: Brief History of USD
date: '2016-11-23 12:08:00 +1000'
background: 'https://source.unsplash.com/random'
categories:
- Technology
tags:
- USD
- CRM
---
# Brief History of USD

USD, CCD, CCA, UII, CCF, AIF…. ever seen so many TLA's in one sentence? Brace yourself, you are in for a ride. Jokes aside, I am going to give you a glimpse of the evolution of USD. Apart from enjoying some technology history, I believe you might be able to make some sense about some of the product features and design decisions you might have had to take during USD implementation
 
Below graph will give you a glimpse of the evolution of Unified Service Desktop from Customer Care Framework. (sort of a TLDR version, (I warn you about TLA's :))

![image](/uploads/2016/11/usdhistory1.png){: .img-fluid}
 
## How it started
It all started somewhere in 2002 from an implementation done by Microsoft Consulting Services for a customer, integrating some of their LOB applications and creating an integrated desktop application housing various LOB applications. After the implementation Microsoft felt that such an application would benefit many, especially call centres and released Contact Centre Framework in year 2003. It was named Contact Centre Framework as it as aimed at contact centres
 
Later Microsoft renamed it to Customer Care Framework realising the potential for the applicability of the product in environments other than contact centres.
 
CCF as it is commonly known was in existence until 2009 and the last official release Customer Care Framework 2009 SP1 QFE was in August 2009. CCF was created as a framework for integrating Line of Business applications in an enterprise, aggregating them in a single agent desktop application and automating the interaction between these applications, thereby reducing the tab switches and/or copy paste between these applications. This helps users to perform certain workflows involving multiple applications, significantly faster.
 
## Architecture of Customer Care Framework
 
Below diagram shows a high-level architecture of Customer Care Framework.
 
![image](/uploads/2016/11/usdhistory2.png){: .img-fluid}
 
The most important architecture component in CCF is AIF, (yeah another TLA) -Application Integration Framework. As said before CCF enables aggregating and automating various Line of Business applications at the user interface level thereby accelerating various business processes a user performs with those applications. Yep, you got it right, Aggregate, Automate and Accelerate was the tag line of the Customer Care Framework. AIF was the component in CCF which enabled this.
 
AIF was implemented on top of Composite UI Application Block (CAB). CAB was an application block from Microsoft Patterns and Practices (P&P) team intended to demonstrate how to create composable applications using windows forms using .NET Framework. CAB had a huge learning curve and it was not popular at all among developers using .NET to create windows forms applications.
 
Another important component in the architecture is the reference implementation. Customer Care Framework was always shipped as a framework, not as an out of box product. This meant customer's buying the framework should implement their own agent desktop application, aggregating various LOB application. This was never a simple process and hence Microsoft shipped source code of a fully functional agent desktop application built using CCF and called it reference implementation.
 
## Changes to Customer Care Framework 2009 SP1 QFE.


### Desktop API
 
As mentioned before creating an agent desktop application using CCF was not an easy task and the reference implementation had a lot of source code. With the release of CCF 2009 SP1 QFE, Microsoft moved some of the components of the reference implementation to be part of CCF framework itself to make the building of agent desktop easier for the developers. This was informally called as desktop API.
 
### WPF support.
 
I must admit, I am not sure about the timelines when WPF support was introduced in CCF. If my memory serves me right WPF support was introduced in CCF 2009 SP1 QFE. The biggest challenge with CCF in moving to WPF or supporting WPF was its dependency on CAB. CAB was created for windows forms. Some smart guy had ported CAB to support WPF and the project was available in codeplex. You can check it out [here][1]. Microsoft decided to use WPFCAB in CCF but did so in most inefficient way. During that time Microsoft had serious restrictions in using open source code in their commercially shipped products. Because of this they could never ship WPFCAB as a part of the product. So, one had to download WPFCAB from codeplex compile and use it to compile the Agent Desktop application.  Moreover, to make WPFCAB compatible with CCF Microsoft had to change some source code inside WPFCAB implementation, however they could never ship it. As a result, anybody wanted to use WPFCAB in Agent Desktop application had to download the source code make the changes compile it. Microsoft documented the steps in an [MSDN][2] article
 
### CCA 
 
CCF as a separate product ceased to exist after the release of CCF 2001 SP1 QFE. CCA stands for Customer Care Accelerator. The below diagram gives a high level of overview of high level architecture of CCA with respect to that of CCF. As you can see the backend database and webservices were replaced by CRM. AIF was renamed to UII (User Interface Integration). Previous reference implementation or AgetDesktop was shipped as an accelerator and was available in codeplex as a free download. However, if you had to use the accelerator you should have CRM license. CCA R2 was released a while later to support CRM 2011 and CRM online. Everything else remained same.
 
![image](/uploads/2016/11/usdhistory3.png){: .img-fluid}
 
### CCD
Popups has always been a problem with CCF. Especially web popups. A lot of CCF and CCA implementations had to implement custom coding to handle popups. And if you remember until version 2011, CRM was full of popups. With the release of CCA, more and more people started integrating CRM and stumbled on this problem of popups. A little while after the release of CCA, Microsoft Consulting Services created a solution for handling popups in CRM and other web applications and provide fist class out of box integration for various CRM entities. As far as CCF or CCA is concerned CRM was treated as any other web application when hosted inside. CCD changed this and made CRM as first class citizens. Incidentally a pivotal feature of USD. CCD was never released to public and was only available for Microsoft Consulting Services projects.
 
### USD
After second release of CCA, CRM team decided to bundle features into the next release of the CRM. They decided to use CCD and package it as USD (Unified Service Desk) as you know in its current form. However, with this release Microsoft had addressed issues with WPFCAB. WPFCAB has been currently rolled under SCSF (Smart Client Software Factory) codeplex project which was released by Microsoft after it released CAB. Yes, you got it right, USD still uses CAB which was created for windows forms and later modified to support WPF. Because of this, a lot of old CCF features work the same way in USD. So, we have come a full circle from CCF to USD and CAB stuck along :)

[1]: http://wpfcab.codeplex.com/
[2]: https://msdn.microsoft.com/en-us/library/ee712804.aspx