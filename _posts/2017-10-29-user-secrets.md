---
layout: post
title: Safe storage of app secrets during development in ASP.NET Core
time: 2017-10-29
---

The title of this article is a direct copy of [this](https://docs.microsoft.com/en-us/aspnet/core/security/app-secrets?tabs=visual-studio) great article on Microsoft's website. The reason I am writing this post is because one should know the options when developping and running [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/) application locally and does not want to commit sensitive data like password, API key, etc... So I am hoping to contribute to spreading the words with that post :)

I highly recommend reading Microsoft's article, it is very informative, but if you want to see how I used Secret Manager in practice then please read on!

##### Secret Manager

The idea behind Secret Manager is to store sensitive data locally and use them when developping and running applications locally. Bear in mind that the data is not encrypted, it is just saved in clear in a file locally.

##### Reference the Secret Manager tool to your ASP.NET Core project

First, add the Secret Manager tool, i.e reference it in the project file you wish to use sensitive data in:
```xml
<ItemGroup>
    ...
    <DotNetCliToolReference Include="Microsoft.Extensions.SecretManager.Tools" Version="2.0.0" />
</ItemGroup>
```

In Visual Studio (VS) saving downloads the NuGet package, in Visual Studio Code (VSCode) `dotnet restore` must be called.


##### Create and Reference a User Secret

The next step is to create a user secret containing the sensitive data and reference it in the project.
<br/>
In VS you can right click on the project file and you should see a menu item called "Manage User Secrets". However, it is more interesting to do it manually at least once to understand how it works.

A user secret is just a JSON file. It must be stored under `~/.microsoft/usersecrets/<userSecretsId>/secrets.json` (`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json` on Windows) where `<userSecretsId>` can be anything.
<br/>
In my case my user secret looks like:
```
// ~/.microsoft/usersecrets/serviceConfigSecretId/secrets.json
{
  "SecretServiceConfig": {
    "address": "https://somewhere-secret.com",
    "key": "super-secret-key"
  }
}
```

Then, the user secret must be referenced in the project file:
```xml
<PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
    <UserSecretsId>serviceConfigSecretId</UserSecretsId>
</PropertyGroup>
```

With that in place, the user secret can now be used in code.


##### Add the User Secret to the Configuration at run time

Ultimately, when my application will be deployed, the sensitive data will be injected via the configuration at start time. So what I want is to be able to add the User Secrets to the configuration at start time but only in development mode. The way (or at least one way) to do it to add the user secret to the C# Configuration object when it is built:
```
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        var builder = new ConfigurationBuilder()
            .SetBasePath(env.ContentRootPath)
            .AddJsonFile("appsettings.json");

        if (env.IsDevelopment())
        {
            builder.AddUserSecrets<Startup>();
        }

        Configuration = builder.Build();
    }
```
An interesting thing to note here is that one can mix configuration sources. In my case, my application configuration comes from two sources: one is the appsettings.json file stored with my code; the other is the data stored in the user secrets. This is super useful!

Once the configuration object is built, it doesn't matter where the data came from, you can access it as usual by calling `Configuration.GetSection("SecretServiceConfig")`. This will return a ConfigurationSection object with all the data.


##### Using Option to access Configuration

The Configuration object can be used directly but a more elegant way to access configuration is using what Microsoft calls the option pattern. It basically helps match configuration section to C# classes. I won't describe how I used it in this post as it is out of scope but it is worth knowing about it, you can check out this [article](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration?tabs=basicconfiguration) about Configuration, they mention it.

<br/>

So Secret Manager is very useful when developping ASP.NET Core applications locally. There are other ways of achieving the same things (like using environment variables) but I like this solution as it is very straighforward to configure. There is only one thing that bothers me: the extra `if` statement in code to execute extra operations in development. I don't like it very much, I prefer when the code path remains the same no matter what environments the code is running in. However, that will do for now!