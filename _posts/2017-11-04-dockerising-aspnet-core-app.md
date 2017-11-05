---
layout: post
title: Dockerising an ASP.NET Core application
date: 2017-11-04
---


A few days ago I started looking at ways to deploy my [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/) application. I looked at [Cloud](https://en.wikipedia.org/wiki/Cloud_computing) solutions but ended up deciding to first focus on containerising my application and only later to look at deployment solutions. In this post I will explain the rationale behind my decision and how I containerised my application.


#### ASP.NET Core deployment to the Cloud

Because my application is an ASP.NET Core service, I naturally looked at [Microsoft Azure](https://azure.microsoft.com). It has some very well integrated solutions to deploy ASP.NET Core applications: in this [tutorial](https://docs.microsoft.com/en-us/aspnet/core/tutorials/publish-to-azure-webapp-using-vs) for instance they show how to do one-click deployment from Visual Studio. In this [one](https://docs.microsoft.com/en-us/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts) they explain how to use Microsoft CI tool VSTS to automatically deploy to Azure. Finally [here](https://blogs.msdn.microsoft.com/benjaminperkins/2017/05/10/deploy-github-source-code-repositories-to-an-azure-app-service/), they describe how to deploy directly from [GitHub](https://github.com/).
<br/>
These are all fine solutions, however, there is too much magic going on to my taste. For instance I couldn't figure out what exactly happens between the developer editing the code and the code being deployed. Obviously the code is built but where? by what? how? Also, what happens if the deployment fails, is the code built again? Can I redeploy the same artifacts?
<br/>
I am sure there are answers somewhere but I couldn't find them so I moved on.

I briefly did a search around [Google Cloud](https://cloud.google.com) and ASP.NET Core and found this good [tutorial](https://codelabs.developers.google.com/codelabs/cloud-app-engine-aspnetcore/#0). I felt like I understood a bit more about what was going on, however, although they mention packaging the app as Docker container at the start, I didn't see it happening anywhere. It looks like the .NET binaries are built and then deployed somewhere where the .NET runtime is installed. It works but I didn't like the idea of delegating control over the environment my application was running in.

And so this is when I realised that what I really wanted was to keep control over my application environment and avoid the following problems:
* missing dependencies
* environment specific problems due to environment discrepancies
* platform incompatibilities

And right now the best way to solve these problems is to package an application into a container. It completely isolates the application and its dependencies, it is environment agnostic, i.e. what runs in dev runs in production, and the only cross-platform problems are down to what runs the container. And I am fine with that.

So I decided to make containerisation part of my own CI process and not delegate it to any cloud services. I will use cloud providers only for container deployment.


#### ASP.NET Core Containerisation

There are a few container technologies available but I am familiar with [Docker](https://www.docker.com/) so I decided to stick with it. Using Docker to containerise an ASP.NET Core application is fairly easy thanks to Docker's [documentation](https://docs.docker.com/) and to Microsoft base image [microsoft/aspnetcore:2.0](https://hub.docker.com/r/microsoft/aspnetcore/). The challenges are more around how to use it. By that I mean there are quite a few decisions to make: what should the container contain? Should the environment configuration be injected? How should configuration or sensitive data be injected? How should it integrate with the Continuous Integration process?
<br/>
Below, I will show how I answered these questions. Bear in mind that I am by no means an expert!

##### Container content

With Docker the container is actually the "thing" running while we talk about building **images**. For now my image contains the binaries and the default appsettings.json configuration that is shared between all environments. My Dockerfile looks very similar to the one on [microsoft/aspnetcore:2.0](https://hub.docker.com/r/microsoft/aspnetcore/) image documentation (Pay extra attention to the *Note on ports*).
<br/>
So what do I do in practice with that? First I build the code and I package it. Then I build the image and I run it.
```
dotnet build
dotnet publish -o somewhere
docker build -t myapp .
docker run -p 1234:80 myapp
```
So very simple.

##### Injecting configuration

Very often applications need extra bits of information in order to run properly: for instance web addresses or API keys to connect to external services. So let's look at how to inject data to the container at start time.
<br/>
As far as I know they are two solutions out of the box. Either use environment variables. Or mount a volume. I couldn't figure out which solution is best so I took the simplest approach for my use case: the data I want to inject is a JSON file so it was simpler to just mount it.

This is the C# code that reads the configuration:
```
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        var builder = new ConfigurationBuilder()
            .SetBasePath(env.ContentRootPath)
            .AddJsonFile("appsettings.json")
            .AddJsonFile("/config/extra_configuration.json"); // the important line
        Configuration = builder.Build();
    }
}
...
```
And this is how I mount and run the container (assuming extra_configuration.json is located at /path/to/sensitive/data/dir).
```
docker run -p 1234:80 -v /path/to/sensitive/data/dir:/config myapp
```

<br/>

So at this stage I am able to locally create a Docker image with my code and run it with different configurations. The process is quite manual so in the next post I will describe how I integrated it with my Continuous Integration process.