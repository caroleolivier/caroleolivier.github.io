---
layout: post
title: Creating a SOAP to JSON converter service
date: 2017-10-19
---

* vs2017
* netcore
* web api project
* install extension: https://marketplace.visualstudio.com/items?itemName=WCFCORETEAM.VisualStudioWCFConnectedService
(review unfair? worked fine for me)
* had to install .netcore tool (https://github.com/dotnet/wcf/issues/2340)
* follow tutorial

* WSDL of nationalrail: https://lite.realtime.nationalrail.co.uk/OpenLDBWS/rtti_2017-02-02_ldb.wsdl

* use appsettings for config https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration?tabs=basicconfiguration

* user user secrets: https://docs.microsoft.com/en-us/aspnet/core/security/app-secrets?tabs=visual-studio
(had to restart VS to see menu manage user secrets)