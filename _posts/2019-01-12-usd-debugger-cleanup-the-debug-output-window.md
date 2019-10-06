---
layout: post
title: "USD Debugger: Cleanup the Debug output window"
subtitle: "USD Debugger: Cleanup the Debug output window"
date: '2019-01-12 12:08:00 +1000'
background: 'https://source.unsplash.com/random'
categories:
- Technology
tags:
- USD
- CRM
---
# USD Debugger: Cleanup the Debug output window

Ever been frustrated by the avalanche of log entries in USD Debug Output window as shown below?

![image](/uploads/2019/01/usddebugger1.png){: .img-fluid}


These log entries are from Microsoft Active Directory Authentication Library (ADAL), which USD uses to connect to Dynamics 365. In order to suppress these log entries use the below code.
    
```csharp    
    protected override void DesktopReady(){
        Microsoft.IdentityModel.Clients.ActiveDirectory.AdalTrace.LegacyTraceSwitch.Level = TraceLevel.Off;base.DesktopReady();
    }
```

![image](/uploads/2019/01/usddebugger2.png){: .img-fluid}

You can create a custom USD hosted control, configure it as a global hosted application and the code will be executed after the USD is loaded. I haven't found a way to suppress these log entries using configuration. I will keep looking and will update this post if I find a solution


  