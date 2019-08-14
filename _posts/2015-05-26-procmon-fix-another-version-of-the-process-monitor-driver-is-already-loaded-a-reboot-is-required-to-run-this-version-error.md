---
layout: post
status: publish
published: true
title: 'procmon : fix &ldquo;Another version of the Process Monitor driver is already
  loaded. A reboot is required to run this version&rdquo; error'
subtitle: 'procmon : fix &ldquo;Another version of the Process Monitor driver is already
  loaded. A reboot is required to run this version&rdquo; error'
date: '2015-05-26 12:09:03 +1000'
background: 'https://source.unsplash.com/random/300x100'
categories:
- Technology
tags:
- procmon
- procexp
- sysinternals
comments: []
---
# procmon : fix “Another version of the Process Monitor driver is already loaded. A reboot is required to run this version” error

I unfortunately had multiple copies of procmon in my machine and after running an older version of the procmon, any attempt to run a newer version resulted in an error message saying "Another version of the Process Monitor driver is already loaded. A reboot is required to run this version". I tried running procmon /terminate without any avail. 

The solution is simple. Navigate to HKCUSysInternalsProcess Monitor registry key. Delete the Process Monitor node or delete all the keys and values under the node. Before doing this you may want to make sure procmon is not running. Running procmon /terminate command is a good option to consider. Restart the machine and you should be able to run the new version of the procmon.

![image](/uploads/2015/05/registry_thumb.png){: .img-fluid}

The reason behind the error is because the procmon*.sys driver was always held by the OS kernel after you first execute the procmon.exe. This looks life a bug because the newer versions of the procmon.exe doesn't seem to do this. You can find out whether the driver is loaded by the kernel by running the pocexp tool.

![image](/uploads/2015/05/procexp_thumb.png){: .img-fluid}

From the above image you can see that PROCMON23.SYS driver is loaded by System process. If you would like to check this in proxexp, make sure you enable the lower pane by pressing Ctrl+L key and customize the content displayed in the lower pane to DLL using the Ctrl+D key. Alternatively you can perform both these operations from the View menu in proxexp

Hope this helps!!!

This entry was posted in [Technology][3] and tagged [procexp][4], [procmon][5], [sysinternals][6] on [May 26, 2015][7] by [arkumar][8]. 

[1]: /uploads/2015/05/registry_thumb.png "registry"
[2]: /uploads/2015/05/procexp_thumb.png "procexp"
[3]: http://iunknownme.com/blog/category/technology/
[4]: http://iunknownme.com/blog/tag/procexp/
[5]: http://iunknownme.com/blog/tag/procmon/
[6]: http://iunknownme.com/blog/tag/sysinternals/
[7]: http://iunknownme.com/blog/2015/05/26/procmon-fix-another-version-of-the-process-monitor-driver-is-already-loaded-a-reboot-is-required-to-run-this-version-error/ "12:09 pm"
[8]: http://iunknownme.com/blog/author/arkumar/ "View all posts by arkumar"
