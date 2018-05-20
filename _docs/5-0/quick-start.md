---
layout: doc-5-0
title: Getting Started
prev_section: what-is-condep
next_section: new
permalink: /docs/5-0/quick-start/
---

There are four simple steps to follow in order to define your configuration, deployment
and/or orchestration using ConDep:

1. Create a Visual Studio project and add the `ConDep` package via `NuGet`.
2. Add a class inheriting from `Runbook` and use the ConDep DSL to execute local and/or remote Operations.
3. Add a `[env].env.json` file (e.g. `dev.env.json`) containing information about your environment to the project.
4. Build your solution, and run `ConDep.exe` to execute your **Artifact(s)**.

### 1. Create a Visual Studio project

Create a new Visual Studio Project in a new Solution. Using the Package Manager
Console (NuGet):

{% highlight powershell %}
install-package condep
{% endhighlight %}

If you want to install a pre-release of ConDep, you can find all of ConDep's pre-release
packages over at myget.com. The source you need to add to NuGet is:
[https://www.myget.org/F/condep/](https://www.myget.org/F/condep/)

<div class="note warning">
	<h2>Avoid mixing ConDep with your existing projects</h2>
  <p>
		We recommend you create and maintain a separate Visual Studio Solution for your
    ConDep code. This will avoid conflicts with other NuGet packages from other projects,
    and help keep your deployment and infrastructure code separate from your applications.
	</p>
</div>

### 2. Define your Runbook

A Runbook represents the configuration, deployment and/or orchestration of one (or more) parts
of your application and its required infrastructure. For a web application for instance,
you want to first create a Runbook for the web server, making sure all required components
are configured and installed. For that you want to have a separate Runbook class (for
instance, MyWebServer). Then you want to deploy your web application with its required
components, like a web application and a windows service, to your web server. For this you
should create two runbook classes (for instance, MyWebApp and MyWindowsService).

In the example below we assume you have a server available already, and want to deploy
an application to it. In a real world example you would want to also configure the server
infrastructure and maybe even bootstrap the server using ConDep.

Add a new class to your project (for example, `MyApp`).

Here's a sample that uses the [Deploy Directory](/docs/5-0/operations/remote/deployment/directory)
Operation inheriting from `Runbook`:

{% highlight csharp %}
public class MyAppDeployment : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.Deploy.Directory(
                                                sourceDir: @"C:\Apps\MyApp", 
                                                destDir: @"E:\Apps\MyApp"));
    }
}
{% endhighlight %}

Here's another example using the DSL to run an operation locally, a [Configuration transform](/docs/5-0/operations/local/transform-config)
Operation:

{% highlight csharp %}
public class MyAppConfigTransformation : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Local.TransformConfigFile(@"C:\MyApp", "web.config", "web.test.config");
    }
}
{% endhighlight %}

> The examples above shows two operations (deploy directory and transform config). See the [Operations](/docs/5-0/operations) section for more details on the 20+ built-in
> Operations.

The ConDep DSL provides different kinds of operations. Above you saw two exmples, Remote and Local operations.

Here is a code snippet that shows what the DSL can offer:
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

The runbook above transforms configs and deploys a Windows service and a directory. All the operations are executed in the same order as in the Runbook.

### 3. Create your Environment file

In order to let ConDep know about the servers in your environments, you need to
create one .json file per environment in the format `[env_name].env.json`.

Example: For your Development environment, you could add a `dev.env.json` file,
and for Production, `prod.env.json`.

> If you're using Visual Studio, remember to set the property `Copy to Output
> Directory` for this file to `Copy if newer`.

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

> See the [Environments](/docs/3-0/environment/) section for more details on environment
> configuration.

### 4. Build the Solution, and execute your operations

To run the operations shown above, build your solution and open a Command Prompt. In
the Command Prompt, change directory (`cd`) to: `[your_project_path]\bin\debug\`. In
this folder, you'll find a copy of `condep.exe`. Run the following command:

{% highlight bat %}
condep.exe deploy [your project assembly name].dll dev MyApp
{% endhighlight %}

For help using this command, run `condep.exe help deploy` or just `condep.exe` to see
all available commands.

### 5. A samples repository

We have made a [sample repository](https://github.com/condep/condep-samples) that contains some good examples on how to use ConDep to configure servers and deploy service and applications. The example also contains some examples on extending ConDep by making your own operations.