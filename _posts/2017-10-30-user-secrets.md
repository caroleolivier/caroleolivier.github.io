---
layout: post
title: Safe storage of app secrets during development in ASP.NET Core
time: 2017-10-30
---

The title of this article is a direct copy of [this](https://docs.microsoft.com/en-us/aspnet/core/security/app-secrets?tabs=visual-studio) great article on Microsoft's website. The reason I am writing this post is because one should know the options that permit you to develop [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/) applications locally without risking committing sensitive data like passwords, API keys etc... So I am hoping to contribute to spreading the words with this post :)

I highly recommend reading Microsoft's article, it is very informative, but if you want to see how I used Secret Manager in practice then please read on!

##### Secret Manager

The idea behind Secret Manager is to store sensitive data locally and use them when developping and running applications locally. Bear in mind that the data is not encrypted, it is just saved in plaintext in a file locally.

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

Secret Manager is great for using locally. I guess it could still be used if the application is deployed on a server and it might even be possible to use it inside a container but it definitely isn't meant to be used in production. I haven't decided where and how my application will be deployed yet, however I know that ultimately I want the sensitive data to be injected and consumed as configuration. Luckily, ASP.NET Core comes with many handy methods to achieve this. In the case of user secrets, the way to do it is the following:
```
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        var builder = new ConfigurationBuilder();

        if (env.IsDevelopment())
        {
            builder.AddUserSecrets<Startup>();
        }

        Configuration = builder.Build();
    }
```
As an illustrative example, it is possible to do the following:
```
    var builder = new ConfigurationBuilder();
        .SetBasePath(env.ContentRootPath)
        .AddJsonFile("appsettings.json")
        .AddEnvironmentVariables()
        .AddUserSecrets();

```

As you can see it is very easy to mix configuration sources, this is super useful!

Once the configuration object is built, it doesn't matter where the data came from, you can access it as usual by calling `Configuration.GetSection("SecretServiceConfig")`. This will return a ConfigurationSection object with all the data.


##### Using Option to access Configuration

The Configuration object can be used directly but a more elegant way to access configuration is using what Microsoft calls the option pattern. It basically helps match configuration sections to C# classes. I won't describe how I used it in this post as it is out of scope but it is worth knowing about it, you can check out this [article](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration?tabs=basicconfiguration) about Configuration, they mention it.

<br/>

So Secret Manager is very useful when developping ASP.NET Core applications locally. There are other ways of achieving the same things (like using environment variables) but I like this solution as it is very straighforward to configure. There is only one thing that bothers me: the code path executed in production versus development is different. I don't like it very much. However, I am not there yet so that will do for now!