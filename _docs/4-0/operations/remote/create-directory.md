---
layout: doc
title: CreateDirectory
permalink: /3-0/remote/create-directory/
version_added: 3.2.0
---

Creates a directory

## Syntax

{% highlight csharp %}
CreateDirectory(string path)
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
			<td>path</td>
			<td>The path to the Directory to create.</td>
		</tr>
	</tbody>
</table>

## Code Example

{% highlight csharp %}
public class MyRemoteArtifact : Artifact.Remote
{
  public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
  {
    server.CreateDirectory(@"C:\temp\MyDirectory");
  }
}
{% endhighlight %}
