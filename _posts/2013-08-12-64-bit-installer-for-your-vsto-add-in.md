---
layout: post
background: 'https://source.unsplash.com/random/300x100'
title: 64-bit Installer for your VSTO add-in
subtitle: Create an Installer for your VSTO add-in
date: '2013-08-12 09:26:32 +1000'
categories:
- Technology
tags:
- VSTO
- Installshield
---
I hope this is my last post on VSTO. I am not an expert in VSTO and I am really excited about the new app model for Office 2013, which lets you create a cleaner powerful extensions for office products like Work, Excel, Outlook and even SharePoint

However, If you are planning to create an installer for your VSTO add-in using the instructions [here](http://msdn.microsoft.com/en-us/library/vstudio/cc442767.aspx#Obtain) and deploy on a 64-bit machine it will not work.This is because the registry key is different for 64-bit machines at the step where you create a .prq XML file and add Visual Studio 2010 Tools for Office Runtime as a pre-requisite. Instead of the XML file snippet, use the following one when you create a .prq file for 64-bit machines

        <SetupPrereq>
        <conditions>
            <condition Type="32" Comparison="2" Path="HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\VSTO Runtime Setup\v4R" FileName="Version" ReturnValue="10.0.40303" Bits="2"></condition>
        </conditions>
        <files>
            <file LocalFile="&amp;lt;ISProductFolder&amp;gt;\SetupPrerequisites\VSTOR\vstor_redist.exe" URL="http://go.microsoft.com/fwlink/?LinkId=140384" CheckSum="b6639489e159b854b6dc43d5cb539043" FileSize="0,40023024"></file>
        </files>
        <execute file="vstor_redist.exe" returncodetoreboot="1641,3010" requiresmsiengine="1">
        </execute>
        <properties Id="Your GUID goes here" Description="This prerequisite installs the most recent version of the Microsoft Visual Studio 2010 Tools for Office Runtime." >
        </properties>

        </SetupPrereq>
