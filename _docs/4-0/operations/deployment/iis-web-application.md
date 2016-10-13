---
layout: doc
title: IisWebApplication
permalink: /3-0/deployment/iis-web-application/
version_added: 3.1.0
---

Works exactly as the [Directory](../directory/) operation, except it will mark the directory as a Web Application in IIS on the remote server. If you emit the `destDir`, ConDep will use the physical path from the Web Site and add the Web Application directly under that path.

## Syntax

{% highlight csharp %}
IisWebApplication(string sourceDir, string webAppName, string webSiteName)
{% endhighlight %}

{% highlight csharp %}
IisWebApplication(string sourceDir, string destDir, string webAppName, string webSiteName)
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
			<td style="white-space: nowrap;">destDir</td>
			<td>Destination directory on the server. If emitted, ConDep will use the physical path from the Web Site and add the Web Application directly under that path.</td>
		</tr>
		<tr>
			<td>webAppName</td>
			<td>Name of the Web Application as it will be registered in IIS</td>
		</tr>
		<tr>
			<td>webSiteName</td>
			<td>Name of an existing Web Site where the Web Application will be put</td>
		</tr>
	</tbody>
</table>

## Code Example
In the below example we use argument name specification to make it easier to read (like `sourceDir:`). This is often useful with methods with multiple parameters, typically of the same type. This is however just a C# feature and optional.

{% highlight csharp %}
public class MyRemoteArtifact : Artifact.Remote
{
  public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
  {
    server.Deploy.IisWebApplication(
                sourceDir: @"C:\MyWebApp",
                destDir: @"E:\WebApps\MyWebApp",
                webAppName: "MyWebApp",
                webSiteName: "MyWebSite");
  }
}
{% endhighlight %}
