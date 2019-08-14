---
layout: post
title: Technical Readiness plan
subtitle: Technical Readiness plan
date: '2013-08-13 12:54:32 +1000'
background: 'https://source.unsplash.com/random/300x100'
categories:
- Technology
tags:
- Readiness
- Cloud
- Node.js
- BigData
comments: []
---
Look at the Google trends graph below showing search trends for different .net framework versions over the last one and half year. On an average 50% of searches are still on .net framework 2.0. While this is not an indication of how many applications are built on different versions of .NET, I am sure that percentage of applications built on earlier versions of .NET Framework (earlier than version 3.0) could be much higher.  
<script type="text/javascript" src="https://ssl.gstatic.com/trends_nrtr/1845_RC03/embed_loader.js"></script> <script type="text/javascript"> trends.embed.renderExploreWidget("TIMESERIES", {"comparisonItem":[{"keyword":".net 1.1","geo":"","time":"2012-01-01 2013-08-31"},{"keyword":".net 2.0","geo":"","time":"2012-01-01 2013-08-31"},{"keyword":".net 3.0","geo":"","time":"2012-01-01 2013-08-31"},{"keyword":".net 3.5","geo":"","time":"2012-01-01 2013-08-31"},{"keyword":".net 4","geo":"","time":"2012-01-01 2013-08-31"}],"category":0,"property":""}, {"exploreQuery":"date=2012-01-01%202013-08-31&q=.net%201.1,.net%202.0,.net%203.0,.net%203.5,.net%204","guestPath":"https://trends.google.com:443/trends/embed/"}); </script> 
So I was not surprised when one of my friends asked me to help him create a readiness plan to update himself from .NET 2.0 to .NET 4.0/4.5 (Could be 5.0 soon) . According to him he gets lost, as soon after he starts looking around what is new in .NET 4.0/4.5. That shouldn’t surprise anybody, a lot has changed in the technology space since 2003. Look at the famous photo to get an indication of the upsurge in smartphone and in general mobile device penetration.

![image](http://claydelk.com/wp-content/uploads/2013/03/NBCNews-St-Perters-Square-Instagram.png){: .img-fluid}

> photo courtesy : instagram

Technology landscape changed a lot, from the avalanche of tables starting with **iPad**, smartphone revolution started with **iPhone,** different providers fighting for cloud supremacy, to the death of **SOAP.**

When I started creating a .NET readiness roadmap (an official term) a.k.a development plan for my friend, I realized how much the technology landscape has become open. Open standards and open source technologies have become so important that no development plan is complete without them.

On second thought it is not really about open source or .NET or any other proprietary technology.  Instead it is about how these advancements influence technologies used to build different layers of an application. So here is my attempt to derive a developer’s roadmap by looking at technology advancements or changes happened at each application layer (User Interface, Services and Persistence) and some of the cross cutting concerns as well.

### User Interface

Device proliferation has created an obvious need for your web pages to be responsive, so that they adapt not only to different browsers, but also to different screen sizes and resolutions. Most of the browser vendors and smartphone makers support HTM5 and CSS3 including IE, Chrome, Firefox, Safari.

I would definitely put basic knowledge of HTML5 and CSS3 in my technical knowledge kitty. You can check out the below pluralsight courses to update yourself with HTML5 and CSS

[HTML5 Fundamentals](http://www.pluralsight.com/training/Courses/TableOfContents/html5-fundamentals-2e)

[CSS3 From Scratch](http://www.pluralsight.com/training/Courses/TableOfContents/css3-from-scratch)

As I write this post, I am sure a new JavaScript library would have been released, maybe another one by the time when you read this post. A myriad of JavaScript libraries starting with jQuery is what made HTML5 based applications popular. I can’t imagine developing a web page without jQuery and Modernizer. If you haven’t dirtied your hands with jQuery until now, you should do it as soon as possible. Again pluralsight has a ton of jQuery courses, but [this one](http://www.pluralsight.com/training/Courses/TableOfContents/jquery-fundamentals) for sure will get you started.

If you are a ASP.NET developer and haven’t checked out [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-music-store) yet, be sure to do that. Along the way you will also encounter many of the JavaScript libraries as well, at the least jQuery for sure.

I am not a CSS expert, hence I rely on [Twitter Bootstrap CSS library](http://pluralsight.com/training/courses/TableOfContents?courseName=bootstrap-introduction&highlight=scott-allen_bootstrap-m1-introduction!scott-allen_bootstrap-m5-javascript!scott-allen_bootstrap-m4-components!scott-allen_bootstrap-m3-everyday!scott-allen_bootstrap-m2-layout#bootstrap-m1-introduction) to create responsive pages.

#### Mobile development

It is no surprise that most companies have a mobile first strategy. With Mobile device proliferation showing no signs of receding in the near future, I would definitely add mobile app development skills to my armory. My personal favorite is [windows phone](http://www.pluralsight.com/training/Courses/TableOfContents/beyond-basics-windows-phone8) and [windows 8 development](http://www.pluralsight.com/training/Courses/TableOfContents/windows8-design-to-delivery), however be sure to check-out other development platforms as well. If you are a .NET developer and interested in cross platform development of mobile applications, be sure to check out [Xamarin](http://xamarin.com/) and [PhoneGap](http://phonegap.com/)

#### Beyond touch

If you are interested in advanced natural UI technologies, you may want to consider learning [Microsoft Kinect SDK](http://www.microsoft.com/en-us/kinectforwindows/develop/developer-downloads.aspx) and [Leap Motion](https://www.leapmotion.com/). Leap Motion even has a c# SDK.

## At the Server’s side

Without any doubt, the most revolutionary thing happened at server side computing, in the recent years is the advent of cloud computing. With Azure and Amazon fighting to serve their customer’s with reduced price and new services and offerings, time to start developing for the cloud is now.

Windows Azure offers three months free trial, thought most of the services you can run locally for development and testing purpose, as a part of SDK.

Amazon does not have a sophisticated client side emulator for their services, but they do offer one year [free trial](http://aws.amazon.com/free/) with some restrictions to the available services

Again, pluralsight has great resources for [Windows Azure](http://www.pluralsight.com/training/Courses#windows-azure).  Though their introductory course on AWS is really good, I found a lot of good resources a their [YouTube channel](http://www.youtube.com/user/AmazonWebServices), especially the below introductory video.

<iframe width="560" height="315" src="https://www.youtube.com/embed/DERzYnthq1s" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### Node.js

One way to look at node.js is as a framework for developing cross platform web applications. If you have been primarily working with .NET and know a little bit of  JavaScript, Node.Js could be your tool to create web applications that will run in both Windows and Linux.  This single threaded, tiny JavaScript  framework is making  waves. So be sure to check out and understand Node.js. Some good tutorials are [here](http://nodetuts.com/index.html) 

#### Take some REST

I found it hard to believe that Roy Fielding had defined REST as early as year 2000, and the creators of SOAP either did not find it or ignored it. If Azure and Amazon can expose their cloud services as RESTful services, you really do not have to think twice for adopting RESTful design for your services. If you are a .NET developer be sure to check out ASP.NET Web API. Some great tutorials are available [here](http://www.asp.net/web-api/overview).

For RESTful API design guidelines check out the [apigee](https://blog.apigee.com/detail/api_design_are_you_a_rest-afarian_or_a_rest_pragmatist/) site. They have some excellent blogs and even YouTube videos.

For a more serious understanding of the architectural principles behind REST, check out the pluralsight [course](http://pluralsight.com/training/Courses/TableOfContents/rest-fundamentals).

During your RESTful journey, if you even stumble upon [oData](http://www.odata.org/), I would suggest you check out why oData is not really REST, before getting excited too much.

## Persisting Data

The famous infographic below shows how much data is generated every minute around world wide web

![image](http://rack.1.mshcdn.com/media/ZgkyMDEyLzA2LzIyLzA2XzU0XzE0XzYzM19maWxlCnAJdGh1bWIJMTIwMHg5NjAwPg/1c7d4fe7){: .img-fluid}


> Image courtesy Mashable

Imagine storing one percentage of this data in a relational database without compromising reliability and scalability. NoSQL (not only SQL) databases originated from this very use case. NoSQL databases sacrifices consistency (eventual consistency against transactional consistency)  in favor of availability, allowing you to scale out indefinitely for storing non-relational data (CAP theorem). Most of the people consider polyglot persistence while designing applications, with a mix of relational and non-relational databases. If you are a .NET developer, consider learning [RavenDB](http://ravendb.net/), which uses LINQ as its querying language. MongoDB and Neo4j are also worth checking out, with the later being a Graph Database, allowing you to store Graph data. One of the interesting things I found about NoSQL databases is that, all of them expose a RESTful API interface.

Great, we have a solution for storing these “BigData”. How do we make some sense out of this? MapReduce is the technology for analyzing this BigData stored in huge clusters of NoSQL databases. If you have started playing with NoSQL databases, RavenDB or MongoDB, you might have already seen that these databases allow you to write Map and Reduce queries.  These days, I haven’t seen a sentence written, without mentioning Hadoop, and most of the people start their NoSQL journey with Hadoop. Though primarily written in Java, Hapdoop has support from Azure and Amazon in the cloud. Here is a great [introduction](http://www.youtube.com/watch?v=qI_g07C_Q5I) to NoSQL by none other than Martin Fowler.

#### ORMs

I haven’t written a SQL query since 2005\. Though I was staying away from SQL and Relational Databases deliberately, so that they don’t influence my NoSQL journey, another reason was ORM’s.  Microsoft’s Entity Framework has come a long way since its inception, though it lacks certain features compared to its counterpart, NHibernate, for e.g. level 2 caching. EF’s fluent API is powerful and easy.  Check out this channel9 [video](http://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Entity-Framework-Tips-and-Tricks) by Entity Framework Guru Julie Lerman

## Security

oAuth is one the latest authorization protocols started by Twitter and popularized by Facebook and other internet majors by. Consuming a service which is implementing oAuth is fairly easy, while implementing oAuth for your service is not a pleasant experience. If you are a **C#** developer check out the [DotNetOpenAuth](http://dotnetopenauth.net/) library.

Claims based authentication was prevalent, well before the “cloud burst”. However I would argue that the popularity of cloud is what made the claims based authentication imperative for a lot applications. Windows Identity Foundation (WIF) , which makes implementing claims based authentication for your applications and services is now a part of the .NET framework itself. Both Azure and Amazon has hosted Access Control Services on the cloud. If you are a .NET developer, you may want to start learning about claims based authentication [here](http://msdn.microsoft.com/en-us/security/aa570351.aspx).

## Programming paradigms

#### Functional Programming

I was listening to one of the [dotnetrocks](http://dotnetrocks.com/) podcasts with Robert C. Martin (@<font style="font-weight: normal">unclebobmartin</font>) about the future of Object-Oriented Programming (if you haven’t checked out dotnetrocks yet, please do so, these guys are amazing, and you will find it difficult to listen to any other podcast, once you listen to dotnetrocks).  Two of the many pearls of wisdom from @<font style="font-weight: normal">unclebobmartin</font> got me thinking. First one was, why would OOP have a future, it is done, go invent something else. Another one was about how variable assignments bring the third dimension of time to your program (a.k.a statefullness), and how this is making difficult to write multi-threaded applications. The answer to both these problems is **Functional Programming**. Though it was not knew, functional programming was always stuck with academia, and never came into commercial software development. @<font style="font-weight: normal">unclebobmartin describes</font> Functional Programming as programming without assignments, that is your variables do not change state when your program is run – a.k.a immutability. Another way to look at functional programming is to think of sending functions to data other than sending data to functions (the way we were taught). If you closely look it, this is what MapReduce is all about. If you have been dealing with BigData and MapReduce, but never came across functional programming, you should definitely check out the concepts

Channel9 has a [Functional Programming Fundamentals](http://channel9.msdn.com/Series/C9-Lectures-Erik-Meijer-Functional-Programming-Fundamentals) series by Dr. Erik Meijer. Do check it out, though the examples are in haskell langauge

F# is the answer if you want to try Functional Programming in Visual Studio. The best way to learn F# is through the [tryfsharp.org](http://tryfsharp.org/) website, though I prefer typing the examples in Visual Studio, than the online editor.

#### Async, Async and await

In my opinion, Task Parallel Library (TPL) and its natural progression async, await pattern, are the two major feature additions to **C#** and **.NET** in the recent times. If you have already programmer using ThreadPool and Asynchronous I/O, you should be able to understand TPL and async/await quite easily. I will try to find a good tutorial and post it here.

## Notable Omissions

WCF – If you have been programming in .NET, you might have noticed that I did not mention WCF at all. Should you learn WCF? Maybe, if you are planning to write SOAP services. How about RESTful services? I wouldn’t implement a RESTFul service in WCF. My choice would be either ASP.NET Web API or Node.js. I have also not mentioned MVVM and WPF. You are sure to encounter MVVM, if you are developing apps for Windows 8 platform.

Happy learning!!!