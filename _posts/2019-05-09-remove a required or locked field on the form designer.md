---
layout: post
title: "CRM Tip: remove a required or locked field on the form designer"
subtitle: "CRM Tip: remove a required or locked field on the form designer"
date: '2019-05-09 12:08:00 +1000'
background: 'https://source.unsplash.com/random'
---

# CRM Tip: remove a required or locked field on the form designer



There are a couple of ways you can remove required or locked fields from the CRM form using the designer. You can hide the field or change the requirement level or use javascript to hide the field at runtime.

Here is a CRM form designer hack which lets you remove the required fields without changing the field properties.

![][7]

Change the number of columns of the tab which contains the required field to two.

![][9]

Move the required field to the second section.

![][11]

Change the number of columns of the tab back to one.

![][13]

CRM will show a warning dialog and will delete all the fields in the second section giving you an empty form

![][15]

[7]: /uploads/2019/05/crmform1.png
[9]: /uploads/2019/05/crmform2.png
[11]: /uploads/2019/05/crmform3.png

[13]: /uploads/2019/05/crmform4.png

[15]: /uploads/2019/05/crmform5.png
