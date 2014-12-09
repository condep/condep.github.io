---
layout: doc
title: Quick Start
prev_section: security
next_section: push-based
permalink: /docs/3-0/quick-start/
---

Quick Start
===========

There are four simple steps to follow in order to define the Operations you want to execute with ConDep:

1. Create a Visual Studio project and add the `ConDep` package via `NuGet`.
2. Add a class inheriting from `Artifact.Local` or `Artifact.Remote` and create a deployment operation using ConDep's DSL.
3. Add a `[env].env.json` file (e.g. `dev.env.json`) containing information about your environment to the project.
4. Build your solution, and run `ConDep.exe` to deploy it.

## 1. Create a Visual Studio project

Create a new Visual Studio Project in a new Solution. Using the Package Manager Console (NuGet):

{% highlight powershell %}
install-package condep
{% endhighlight %}

If you want to install a pre-release of ConDep, add `-pre` to the end of the command.

<div class="note warning">
	<h2>Avoid mixing ConDep with your existing projects</h2>
  <p>
		We recommend you create and maintain a separate Visual Studio Solution for your ConDep code. This will avoid conflicts with other NuGet packages from other projects, and help keep your deployment and infrastructure code separate from your applications.
	</p>
</div>

## 2. Define your Operations

Add a new class to your project (for example, `MyApp`).

Most of the time you want to run operations remotely on the server. In these cases, inherit from the `Artifact.Remote`. In cases where you need **also** to run operations locally (like transforming a .NET config file), inherit from `Artifact.Local` instead. You can still use `Artifact.Local` and use remote operations, but you need to call into `local.OnRemote`.

Here's a sample that uses the [Deploy Directory](/docs/3-0/operations/deployment/directory/) Operation
inheriting from `Artifact.Remote`:

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

Here's the same Directory Deployment Operation when inheriting from `Artifact.Local`;
this includes a local [Transform Configuration](/docs/3-0/operations/local/transform-config/) Operation:

{% highlight csharp %}
public class MyApp : Artifact.Local
{
    public override void Configure(IOfferLocalOperations onLocalMachine, ConDepSettings settings)
    {
        onLocalMachine.TransformConfigFile(
            configDirPath: @"C:\Apps\MyApp\",
            configName: "myapp.config",
            transformName: "myapp." + settings.Options.Environment + ".config"
        );

        onLocalMachine.ToEachServer(server =>
            server.Deploy.Directory(
                sourceDir: @"C:\Apps\MyApp",
                destDir: @"E:\Apps\MyApp"
            ));
    }
}
{% endhighlight %}

> See the [Operations](/docs/3-0/operations/) section for more details on the 20+ built-in Operations.

## 3. Create your Environment file

In order to let ConDep know about the servers in your environments, you need to create one .json file per environment in the format `[env_name].env.json`.

Example: For your Development environment, you could add a `dev.env.json` file, and for Production, `prod.env.json`.

> If you're using Visual Studio, remember to set the property `Copy to Output Directory` for this file to `Copy if newer`.

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

> See the [Environments](/docs/3-0/environment/) section for more details on environment configuration.

## 4. Build the Solution, and execute your operations

To run the operations shown above, build your solution and open a Command Prompt. In the Command Prompt, change directory (`cd`) to: `[your_project_path]\bin\debug\`. In this folder, you'll find a copy of `condep.exe`. Run the following command:

{% highlight bat %}
condep.exe deploy [your project assembly name].dll dev MyApp
{% endhighlight %}

For help using this command, run `condep.exe help deploy` or just `condep.exe` to see all available commands.
