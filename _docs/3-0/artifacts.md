---
layout: doc
title: Artifacts
permalink: /docs/3-0/artifacts/
---

Working with Artifacts
======================

Artifacts is what instrument the Operations you use in ConDep to automate your deployments and/or infrastructure.

There are two Artifact classes available to use in ConDep:

* Artifact.Local
* Artifact.Remote

## Artifact.Local

Use this artifact only if you have any local operations to execute (like transforming .NET config files), if not it's more convenient to use `Artifact.Remote`. You can still access the remote DSL from `Artifact.Local` through the `ToEachServer()` method. Here's an example using a local operation and a remote operation:

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
