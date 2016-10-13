---
layout: doc-4-0
title: Directory
permalink: /docs/4-0/operations/deployment/directory
version_added: 3.1.0
---

## Directory
Will deploy local source directory to remote destination directory. This operation does dot just copy directory content, but synchronize the the source folder to the destination. If a file already exist on destination, it will be updated to match source file. If a file exist on destination, but not in source directory, it will be removed from destination. If a file is readonly on destination, but read/write in source, destination file will be updated with read/write.

### Syntax

{% highlight csharp %}
Directory(string sourceDir, string destDir)
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
			<td>Destination directory on the server</td>
		</tr>
	</tbody>
</table>

### Code Example

{% highlight csharp %}
public class MyRemoteArtifact : Artifact.Remote
{
  public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
  {
    server.Deploy.Directory(@"C:\web\myapp", @"E:\Web\MyApp");
  }
}
{% endhighlight %}
