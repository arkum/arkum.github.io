---
layout: post
title: Extract Team Foundation settings from Team Foundation project add-in
subtitle: Extract Team Foundation settings from Team Foundation project add-in
date: '2013-08-12 06:20:10 +1000'
background: 'https://source.unsplash.com/random/300x100'
categories:
- Technology
tags:
- VSTO
---
One of my project manager friends approached me with a request recently. He wanted a "working" project add-in, that will help him sync certain attributes of the Project tasks with the corresponding work items in Team Foundation server. According to him, the built-in TFS add-in doesn’t work half the time, and when it works, it completely screws up the project file, making it unusable.

Storing the configuration settings for this add-in posed some interesting problems for me. Initially I started out by storing the Team Foundation settings, URL, Team Project etc. in a .config file, thought I felt that this was not a good design. Since these configuration settings will be different for each project file (.mpp), storing these in a configuration file is definitely not a good idea.  I started exploring a different path. If Team foundation Add-in for Microsoft Porject (for that matter, excel also) is storing these settings for each project file, why can’t we read those settings instead of designing our own configuration storage settings

The below diagram shows the custom properties of a Microsoft Project file.

[![image](/uploads/2013/08/image_thumb.png "image")](/uploads/2013/08/image.png) 

In the above diagram you can see some custom properties with the name starting “VS Team System Data DO NOT EDIT” Team foundation add-in stores various configuration settings against these property names. However the values against each of these property names are Base64 encoded. In fact, Team foundation add-in constructs the settings as single xml file, encodes it, splits it into thirty odd segments (as you can see from the number of property names) and stores them as custom property values  

### Code to read custom properties

In order to read these settings, you have to combine these settings, decode and uncompress, which will   produce a single monolithic xml, containing all the configuration settings. Once you have the xml loaded into memory, you can use Linq to XML to read individual configuration settings like Team foundation server name, Team foundation project name etc.

Combine all the custom “VS” property values into a string builder

{% highlight csharp %} 
project = app.ActiveProject;
if (project == null) return;
var properties = (DocumentProperties)app.ActiveProject.CustomDocumentProperties;

string VSTSSystemData = "VS Team System Data DO NOT EDIT";
int propCount = 0;
try
{
    propCount=(int)properties[VSTSSystemData].Value;
}
catch (ArgumentException argex)
{
    //throw new ApplicationException("Project is not connected to TFS");
    MessageBox.Show("Project is not connected to TFS");
    return;
}
StringBuilder sb = new StringBuilder();

for (int i = 0; i < propCount; i++)
{
    sb.Append((string)properties[VSTSSystemData + i].Value);
}
{% endhighlight %} 

Decode the base 64 bit encoded string

{% highlight csharp %} 
char[] chars = sb.ToString().ToCharArray();
byte[] encodedDataAsBytes = System.Convert.FromBase64String(sb.ToString());
{% endhighlight %} 

Before you load the decoded string to an XML document, you have to omit the first 8 bits of the array. These 8 bits store some settings specific to the encoding and compression.

{% highlight csharp %} 
byte[] result = new byte[encodedDataAsBytes.Length - 8];
Array.Copy(encodedDataAsBytes,8, result, 0, encodedDataAsBytes.Length - 8);
MemoryStream s = new MemoryStream(result);
{% endhighlight %} 

Now you have compressed data in a memory stream. You can deflate this memory stream and load the it into a XDocument to access the various Team Foundation configuration settings.

{% highlight csharp %} 
using (var df = new System.IO.Compression.DeflateStream(s, System.IO.Compression.CompressionMode.Decompress, true))
{
    var doc = XDocument.Load(df);
    var tfsservernode = (from x in doc.Descendants("V")
                where x.Attribute("n").Value == "namespaceUrl"
                select x).FirstOrDefault();
    if (tfsservernode == null) { MessageBox.Show("Could not find TFS server name"); return; }
    tfsservername = tfsservernode.Value;
    if (string.IsNullOrEmpty(tfsservername)) { MessageBox.Show("TFS server name is empty"); return; } 
    var projectnode = (from x in doc.Descendants("V")
                       where x.Attribute("n").Value == "project"
                         select x).FirstOrDefault();
    projectname = projectnode.Value;
}
{% endhighlight %} 

> Please note, the above steps apply to Microsoft Project 2010. However the format, Team foundation add-in, encodes and compresses these settings are undocumented and proprietary. Please use these at your own risk