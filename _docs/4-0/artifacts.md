---
layout: doc-4-0
title: Artifacts
prev_section: environment
next_section: condep-exe
permalink: /docs/4-0/artifacts/
---

Artifacts is what instrument the Operations in the ConDep DSL to automate your
deployments and/or infrastructure.

There are two Artifact classes available to use in ConDep:

* Artifact.Local
* Artifact.Remote

## Artifact.Local

Use this artifact only if you have any local operations to execute (like
transforming .NET config files), if not it's more convenient to use `Artifact.Remote`.
You can still access the remote DSL from `Artifact.Local` through the `ToEachServer()`
method. Here's an example using a local operation and a remote operation:

{% highlight csharp %}
public class MyApp : Artifact.Local
{
    public override void Configure(IOfferLocalOperations onLocalMachine, ConDepSettings settings)
    {
        onLocalMachine.TransformConfigFile(
            configDirPath: @"C:\Apps\MyApp\",
            configName: "myapp.config",
            transformName: "myapp.dev.config"
        );

        onLocalMachine.ToEachServer(server =>
            server.Deploy.Directory(
                sourceDir: @"C:\Apps\MyApp",
                destDir: @"E:\Apps\MyApp"
            ));
    }
}
{% endhighlight %}

## Artifact.Remote

If you only need to access remote operations, use this Artifact.

{% highlight csharp %}
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

## Tagging Artifact with Tier

If you have defined Tiers in your [Environment](../environment/) configuration you
can tag your Artifacts with which Tier it belongs in. ConDep has defined an
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

## Adding dependencies between Artifacts

Artifacts can depend on other Artifacts. In the example below the MyApp Artifact
depends on MyServer Artifact. ConDep will then make sure to execute the MyServer
Artifact before the MyApp Artifact.

{% highlight csharp %}
public class MyApp : Artifact.Remote, IDependOn<MyServer>
{
    public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
    {
      ...
    }
}
{% endhighlight %}

The execution order of the above example will be:

`MyServer Artifact` <span class="glyphicon glyphicon-arrow-right" aria-hidden="true"></span>
`MyApp Artifact`

If you depend on Artifacts that has their own dependencies, those Artifact dependencies
will be executed prior to your Artifact.

You can also define multiple Artifact dependencies like this:

{% highlight csharp %}
public class MyApp : Artifact.Remote, IDependOn<MyServer>, IDependOn<MyWinService>>
{
    public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
    {
      ...
    }
}
{% endhighlight %}

The execution order of the above example will be:

`MyServer Artifact` <span class="glyphicon glyphicon-arrow-right" aria-hidden="true"></span>
`MyWinService Artifact` <span class="glyphicon glyphicon-arrow-right" aria-hidden="true"></span>
`MyApp Artifact`

<div class="note warning">
	<h2>Use with care</h2>
  <p>
	Having lots of Artifact dependencies will add complexity and potentially make it impossible to know the actual execution order of Artifacts, so don't overuse.
	</p>
</div>
