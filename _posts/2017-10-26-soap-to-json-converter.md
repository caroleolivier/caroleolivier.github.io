---
layout: post
title: Creating a SOAP to JSON converter service
date: 2017-10-26
---

I recently had to consume a SOAP based service from within JavaScript code. To solve that problem I decided to write a separate service acting as a SOAP to JSON converter. In this post I explain why and how I did it.

#### The problem

I am currently writing a web application to display train data and one of the data source I am using is a SOAP based web service. SOAP stands for Simple Object Access Protocol, it is a protocol to exchange data over [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) using [XML](https://en.wikipedia.org/wiki/XML) as message format. Here is a good example from [Wikipedia](https://en.wikipedia.org/wiki/SOAP#Example_message_.28encapsulated_in_HTTP.29):

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

One of the great thing with SOAP based web service is that they often (they should really) come with a WSDL contract.
> An WSDL document describes a web service. It specifies the location of the service, and the methods of the service
> [WSDL](https://www.w3schools.com/xml/xml_wsdl.asp) 

[This](https://lite.realtime.nationalrail.co.uk/OpenLDBWS/rtti_2017-02-02_ldb.wsdl) is for instance the WSDL document that comes with the SOAP web service I used in my project. You wouldn't want a human being reading that but IDEs such as Visual Studio can easily consume such contract and automatically create interface and classes to consume the service. It is super powerful and makes using SOAP service very easy.

Knowing that, I could still have decided to consume the data source from JavaScript. There are libraries that help with consuming XML and tools that help with reading WSDL. However, a couple of reasons made me pick another solution. First, I wanted to keep my client application super simple and not add too much code for API request/response processing. I use the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) and it is extremely easy to make simple GET HTTP call and process JSON data. Second, I knew how easy it would be to connect to a SOAP web service using WCF in Visual Studio. Finally, I had been meaning to create a REST API for my web application to act as a layer between my web application and all the data sources I use to display data.

So I settled for ASP.NET Core using WCF for connecting to the SOAP web service.


#### In practice

In practice it took a little while to get everything up and running as I had to install a bunch of libraries and softwares. However, this is a one-time step so I think it is worth it.

I installed the following tools:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)
* [.NET Core 2.0.0](https://www.microsoft.com/net/download/core)
* [WCF Web service provider](https://marketplace.visualstudio.com/items?itemName=WCFCORETEAM.VisualStudioWCFConnectedService)

Notes:
<br/>
If you are on Mac, you may need to update your OS to Sierra to install the latest VS2017. I remember having some problems with Xcode although I wasn't directly using it as far as I am aware.
<br/>
Make sure you install at least of VS2017 15.5 on Windows else the WCF web service extensions doesn't work. Well it does but you need to install .NET Core 1.0-1.1 development tools. They fixed it though, see [here](https://github.com/dotnet/wcf/issues/2340).

After the installation step, I then created an ASP.NET Core project, added a service reference and started consuming the SOAP web service. I won't describe how to do that here as there are plenty of good tutorials on the Internet (For WCF Connected service you can check out the official installation web page [here](https://marketplace.visualstudio.com/items?itemName=WCFCORETEAM.VisualStudioWCFConnectedService) and for ASP.NET Core [here](https://docs.microsoft.com/en-us/aspnet/core/tutorials/index))


So to summarise, I wanted to consume a SOAP web service from JavaScript. Because I didn't want to add too much complexity to the web client and because creating an isolated API the client could talk to seemed like a good idea I decided to create an ASP.NET Core service and leverage Visual Studio ability to easily add references to SOAP web services.
