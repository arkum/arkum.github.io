---
layout: post
title: Installing Windows Live Writer 2012 on Windows 10 Technical Preview
subtitle: Installing Windows Live Writer 2012 on Windows 10 Technical Preview
date: '2015-04-16 10:32:06 +1000'
background: 'https://source.unsplash.com/random/300x100'
categories:
- Technology
tags:
- live writer
- blogging
---
Windows Live writer is my primary blog writing tool. I had tried many alternatives including Scribe – the Firefox add-on, various online blog editors and various desktop ones. However I kept going back to Live Writer and Kept installing it on every machine I use. However installing Live Writer on the recent Windows 10 Technical Preview 10049 build was nothing less than nightmarish. Once you start the installation, at around 30% my machine hangs. None of the control keys will work and I had to power down the machine by keeping the power button pressed. Surprisingly no log files or crash dumps were written by the installer.I tired capturing the crash dump using procdump without any luck.

I captured the log file by running the installer from the command line with the /log:. From the log file I could see that the machine hangs at the point where it tries to verify the signature of some downloaded file.</log>

[![writererror](/uploads/2015/04/writererror_thumb.png "writererror")](/uploads/2015/04/writererror.png)

Windows Live writer installation downloads a bunch of files which are windows updates and those updates doesn’t seem to be working well with the Windows 10 Technical Preview. I poked around the command line switches and found that there is an option to disable checking for updates. Finally below is the command line which worked for me.

```console
wlsetup-all.exe    /AppSelect:Writer /q /log:C:\temp\Writer.Log /noMU /noHomepage /noSearch
```

/noMU is the switch to control windows updates.
