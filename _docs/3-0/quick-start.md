---
layout: doc
title: Quick Start
prev_section: security
next_section: push-based
permalink: /docs/3-0/quick-start/
---

Quick Start
===========

There are four simple steps to follow in order to define the operations you want to execute with ConDep:

1. Create and configure you Visual Studio project
2. Create a class inheriting from Artifact.Local or Artifact.Remote to get access to the ConDep's DSL
3. Create a [env].env.json file (e.g. dev.env.json) containing information about your environment
4. Run ConDep.exe pointing to the two above

## 1. Create Visual Studio project

Create a new Visual Studio Project in a new Solution. Using Package Manager Console (NuGet) do:

{% highlight powershell %}
install-package condep
{% endhighlight %}

If you want to install a pre-release of ConDep, add `-pre` to the end of the command

<div class="note warning">
	<h2>Keep your ConDep code in a separate Visual Studio Solution</h2>
  <p>
		We recommend you create and maintain a separate Visual Studio Solution for your ConDep code. This will avoid conflicts with other NuGet packages from other projects and help keep your deployment and infrastructure code separate from your applications(s).
	</p>
</div>

## 2. Defining your operations

Most of the time you want to do operations remotely on the server. In these cases use the `Artifact.Remote`. In cases where you also need to do operations locally (like transforming a .NET config file), use `Artifact.Local`. You can still use `Artifact.Local` and use remote operations, but you need to call into local.OnRemote.

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

Here's the same operation with `Artifact.Local` and config transform:

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

## 3. Create your Environment file

In order to let ConDep know about the servers in your environments, you need to create one .json file per environment in the format `[env_name].env.json`. Example `dev.env.json` for your Dev environment. Note that using Visual Studio set the Property `Copy to Output Directory` to `Copy if newer`.

{% highlight json %}
{
  "Servers" :
  [
    {
      "Name" : "jat-web01"
    }
  ],
  "DeploymentUser": 
  {
    "UserName": "Administrator",
    "Password": "********"
  }
}
{% endhighlight %}

## 4. Execute your operations

To Execute the above, build your solution and open a command window changing directory (`cd`) to: `[your project path]\bin\debug\`. In this folder you'll find condep.exe. Run the following command:

{% highlight console %}
condep.exe deploy [your project assembly name].dll dev MyApp
{% endhighlight %}

For help using this command, run `condep.exe help deploy` or just `condep.exe` to see all available commands.

