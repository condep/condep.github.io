---
layout: doc
title: File
permalink: /3-0/deployment/file/
version_added: 3.1.0
---

Will deploy local file and its attributes to remote server. If the file already exist on the server and either the file or its attributes are different, it will be updated to reflect the file on client.

## Syntax

{% highlight csharp %}
File(string sourceFile, string destFile)
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
			<td>sourceFile</td>
			<td>Source file on the client</td>
		</tr>
		<tr>
			<td>destFile</td>
			<td>Destination file on the server</td>
		</tr>
	</tbody>
</table>

## Code Example

{% highlight csharp %}
public class MyRemoteArtifact : Artifact.Remote
{
  public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
  {
    server.Deploy.File(@"C:\web\myapp\myfile.exe", @"E:\Web\MyApp\myfile.exe");
  }
}
{% endhighlight %}
