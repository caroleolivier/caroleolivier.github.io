---
layout: post
title: Creating a SOAP to JSON converter service
date: 2017-10-28
---

I recently had to consume a SOAP based web service from within JavaScript code. To solve that problem I decided to write a separate service acting as a SOAP to JSON converter. In this post I explain why and how I did it.

#### The problem

I am currently writing a web application to display train data and one of the data source I am using is a SOAP based web service. SOAP stands for Simple Object Access Protocol, it is a protocol to exchange data over [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) using [XML](https://en.wikipedia.org/wiki/XML) as message format.
SOAP can be very powerful but is not as easy to use out of the box as is JSON using JavaScript.
<br/>
Here is a good example from [Wikipedia](https://en.wikipedia.org/wiki/SOAP#Example_message_.28encapsulated_in_HTTP.29):

```xml
    POST /InStock HTTP/1.1
    Host: www.example.org
    Content-Type: application/soap+xml; charset=utf-8
    Content-Length: 299
    SOAPAction: "http://www.w3.org/2003/05/soap-envelope"
    <?xml version="1.0"?>
    <soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope" 
    xmlns:m="http://www.example.org/stock/Manikandan">
    <soap:Header>
    </soap:Header>
    <soap:Body>
        <m:GetStockPrice>
        <m:StockName>GOOGLE</m:StockName>
        </m:GetStockPrice>
    </soap:Body>
    </soap:Envelope>
```
As you can imagine parsing the message with bare JavaScript wouldn't be very fun.

However, one of the great thing with SOAP based web service is that they often (they should really) come with a WSDL contract.
> An WSDL document describes a web service. It specifies the location of the service, and the methods of the service
> [WSDL](https://www.w3schools.com/xml/xml_wsdl.asp) 

[This](https://lite.realtime.nationalrail.co.uk/OpenLDBWS/rtti_2017-02-02_ldb.wsdl) is for instance the WSDL document that comes with the SOAP web service I used in my project. You wouldn't want a human being reading it but IDEs such as Visual Studio (for instance see [here](https://msdn.microsoft.com/en-us/library/ff512390.aspx)) can easily process such contract and automatically create interface and classes to consume the service. It is super powerful and makes using SOAP service very easy.

Knowing that, I could still have decided to call the service using JavaScript. There are libraries that help with reading SOAP based message and tools that help with reading WSDL. However, a couple of reasons made me pick another solution.
<br/>
First, I wanted to keep my client application super simple and not add too much code for API request/response processing. I use the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) and it is extremely easy to make simple GET HTTP call and process JSON data. Second, I knew how easy it would be to connect to a SOAP web service using WCF in Visual Studio. Finally, I had been meaning to create a [REST API](https://en.wikipedia.org/wiki/Representational_state_transfer) for my web application to act as a layer between my web application and all the data sources I depend on.

So I settled for [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/) using WCF for connecting to the SOAP web service.


#### In practice

In practice it took a little while to get everything up and running as I had to install a bunch of libraries and softwares. However, this is a one-time step so I think it is worth it.

I installed the following tools:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)
<br/>It is the best tool (in my opinion) for creating .NET application as it comes with many tools and extensions.
* [.NET Core 2.0.0](https://www.microsoft.com/net/download/core)
<br/>I wanted to use the latest .NET and I don't know what platform I am going to deploy to yet so I used .NET Core
* [WCF Web service provider](https://marketplace.visualstudio.com/items?itemName=WCFCORETEAM.VisualStudioWCFConnectedService)
<br/>This is the extension to VS that helps with adding web service reference

##### Notes
If you are on Mac, you may need to update your OS to Sierra to install the latest VS2017. I remember having some problems with Xcode although I wasn't directly using it as far as I am aware.
<br/>
Make sure you install at least of VS2017 15.5 on Windows else the WCF web service extensions doesn't work. Well it does but you need to install .NET Core 1.0-1.1 development tools. They fixed it though, see [here](https://github.com/dotnet/wcf/issues/2340).

After the installation step, I created an ASP.NET Core project, added a service reference and could very quickly connect and call the SOAP web service. I won't describe how to do it here as there are plenty of good tutorials on the Internet (For WCF Connected service you can check out the official installation web page [here](https://marketplace.visualstudio.com/items?itemName=WCFCORETEAM.VisualStudioWCFConnectedService) and for ASP.NET Core [here](https://docs.microsoft.com/en-us/aspnet/core/tutorials/index)). 

<br/>
Wrapping up what I did: I needed to connect to a new datasource which was a SOAP web service. To achieve this I created a new REST API service acting as a layer between my client application and the data source. I implemented the REST API service with ASP.NET Core and I used Visual Studio WCF Web Service Reference Provider extension to easily connect to the SOAP web service.
<br/>
And voilà!