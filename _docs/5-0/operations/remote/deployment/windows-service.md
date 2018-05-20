---
layout: doc-5-0
title: WindowsService
permalink: /docs/5-0/operations/remote/deployment/windows-service
version_added: 3.1.0
---

Will deploy the given Windows Service to remote server.

## Syntax

{% highlight csharp %}
WindowsService(
  string serviceName,
  string displayName,
  string sourceDir,
  string destDir,
  string relativeExePath
)
{% endhighlight %}
{% highlight csharp %}
WindowsService(
  string serviceName,
  string displayName,
  string sourceDir,
  string destDir,
  string relativeExePath,
  Action<IOfferWindowsServiceOptions> options
)
{% endhighlight %}

<table>
	<thead>
		<tr>
			<th>Parameter</th>
			<th>Description</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>serviceName</td>
			<td>Name of the service</td>
		</tr>
		<tr>
			<td>displayName</td>
			<td>Display name of the service</td>
		</tr>
		<tr>
			<td>sourceDir</td>
			<td>The client directory where the files for the service are located</td>
		</tr>
		<tr>
			<td>destDir</td>
			<td>The destination directory where the service will be deployed to the server</td>
		</tr>
		<tr>
			<td>relativeExePath</td>
			<td>The path or just the name (if located on root) of the executable for the service</td>
		</tr>
		<tr>
			<td>options</td>
			<td>See <a href="../../options/IOfferWindowsServiceOptions/">IOfferWindowsServiceOptions</a></td>
		</tr>
	</tbody>
</table>

## Code Example

{% highlight csharp %}
 public class MyRemoteOperation : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.Deploy.WindowsService(
                serviceName: "MyService",
                displayName: "My Service",
                sourceDir: @"C:\services\myservice",
                destDir: @"E:\services\myservice",
                relativeExePath: @"myservice.exe",
                options: opt => opt
                    .UserName(@"mydomain\myuser")
                    .Password("my$uper$ecretP@ssw0rd")
            ));
    }
}
{% endhighlight %}
