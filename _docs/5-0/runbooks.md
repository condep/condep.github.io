---
layout: doc-5-0
title: Runbooks
prev_section: environment
next_section: condep-exe
permalink: /docs/5-0/artifacts/
---

Runbooks is what instrument the Operations in the ConDep DSL to automate your
deployments and/or infrastructure.

The ConDep DSL offers several kinds of operations:

* dsl.Local.
* dsl.Remote(...)
* dsl.RemoteAsync(...)
* dsl.LoadBalance(...)

## A Runbook Example

Here is a code snippet that shows what the DSL can offer, with some usefull comments:
{% highlight csharp %}
public class MyEnviroment : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        //Local operations are executed locally on the machine that runs ConDep.exe.
        dsl.Local.TransformConfigFile(@"C:\MyService", "app.config", "app.test.config");

        //Remote operations typically do something on a remote machine. Deploys something, configures something etc.
        dsl.Remote(remote => remote.Deploy.WindowsService(
                                                serviceName: "MyService", 
                                                displayName: "MyService", 
                                                sourceDir: @"C:/Services/MySevice", 
                                                destDir: @"E:/Services/MyService",
                                                relativeExePath: @"E:/Services/MyService/MyService.exe"));

        //Another local operation                
        dsl.Local.TransformConfigFile(@"C:\MyWebApp", "web.config", "web.test.config");

        //LoadBalance does remote operations, but allows you to loadbalance while deploying See section about load balancing for more info.    
        dsl.LoadBalance(LoadBalancerMode.Sticky, "MyWebFarm", remote => remote.Deploy.Directory(
                sourceDir: @"C:\Apps\MyApp",
                destDir: @"E:\Apps\MyApp"));   

        // RemoteAsync operations are, drumrole, async.
        dsl.RemoteAsync(remote => remote.Deploy.WindowsService(
                                                serviceName: "MyService", 
                                                displayName: "MyService", 
                                                sourceDir: @"C:/Services/MySevice", 
                                                destDir: @"E:/Services/MyService",
                                                relativeExePath: @"E:/Services/MyService/MyService.exe")); 
    }
}
{% endhighlight %}

The runbook above transforms configs and deploys a Windows service and a directory. All the operations are executed in the same order as in the Runbook, using direct execution. Reed more about that in [What's new](/docs/5-0/new/)

## Tagging Runbook with Tier

If you have defined Tiers in your [Environment](../environment/) configuration you
can tag your Runbooks with which Tier it belongs in. ConDep has defined an
enumeration with the three most common Tiers (Web, Application and Database), but
you can name them whatever you want by using a String instead.

{% highlight json %}
{
  "Tiers":
  [
    {
      "Name" : "Web",
      "Servers" :
      [
        {
          "Name" : "web01.prod.local"
        },
        {
          "Name" : "web02.prod.local"
        }
      ]
    },
    {
      "Name" : "Application",
      "Servers" :
      [
        {
          "Name" : "app01.prod.local"
        },
        {
          "Name" : "app02.prod.local"
        }
      ]
    }
  ],
  "DeploymentUser":
  {
    "UserName": "Administrator",
    "Password": "ReallySecureP@$$w0rd :-)"
  }
}
{% endhighlight %}

{% highlight csharp %}
[Tier(Tier.Web)]
public class MyApp : Artifact.Remote
{
    public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
    {
        server.Deploy.Directory(
            sourceDir: @"C:\Apps\MyApp",
            destDir: @"E:\Apps\MyApp"
        );
    }
}
{% endhighlight %}

{% highlight csharp %}
[Tier(Tier.Application)]
public class MyService : Artifact.Remote
{
    public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
    {
        server.Deploy.Directory(
            sourceDir: @"C:\Services\MyService",
            destDir: @"E:\Services\MyService"
        );
    }
}
{% endhighlight %}

The above code will only execute its Operations on Servers located under the associated
Tier. The `MyApp` Artifact will deploy to `web01.prod.local` and `web02.prod.local`,
while `MyService` Artifact will use `app01.prod.local` and `app02.prod.local`.

## Adding dependencies between Runbooks

Runbooks can depend on other Runbooks. In the example below the MyApp Runbook
depends on MyServer Runbook. If you want to make sure that the MyServer Runbook runs before the MyApp Runbook, just run MyServer within MyApp:

{% highlight csharp %}
public class MyServer : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        //Configure server here
    }
}

public class MyApp : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        Runbook.Execute<MyServer>(dsl, settings);
        //Deploy MyApp here
    }
}
{% endhighlight %}

The execution order of MyApp in the example above will be as it is in the runbook 