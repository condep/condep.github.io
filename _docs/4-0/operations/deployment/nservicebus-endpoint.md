---
layout: doc
title: NServiceBusEndpoint
permalink: /3-0/deployment/nservicebus-endpoint/
version_added: 3.1.0
---

Exactly the same as the [WindowsService](../windows-service/) operation, only tailored for NServiceBus.

## Syntax

{% highlight csharp %}
NServiceBusEndpoint(string sourceDir, string destDir, string serviceName, string profile)
{% endhighlight %}
{% highlight csharp %}
NServiceBusEndpoint(string sourceDir, string destDir, string serviceName, string profile, Action<IOfferWindowsServiceOptions> options)
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
			<td>sourceDir</td>
			<td>Source directory on the client</td>
		</tr>
		<tr>
			<td>destDir</td>
			<td>Destination directory on the server.</td>
		</tr>
		<tr>
			<td>serviceName</td>
			<td>Name of the NServiceBase endpoint</td>
		</tr>
		<tr>
			<td>profile</td>
			<td>The NServiceBus profile to use</td>
		</tr>
		<tr>
			<td>options</td>
			<td>See <a href="../../options/IOfferWindowsServiceOptions/">IOfferWindowsServiceOptions</a></td>
		</tr>
	</tbody>
</table>

## Code Example

{% highlight csharp %}
public class MyRemoteArtifact : Artifact.Remote
{
  public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
  {
    server.Deploy.NServiceBusEndpoint(
      sourceDir: @"C:\services\myservice",
      destDir: @"E:\services\myservice",
      serviceName: "MyService",
      profile: "NServiceBus.Production",
      options: opt => opt
        .UserName(@"mydomain\myuser")
        .Password("my$uper$ecretP@ssw0rd")
    );
  }
}
{% endhighlight %}
