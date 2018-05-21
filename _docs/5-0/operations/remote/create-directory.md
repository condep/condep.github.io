---
layout: doc-5-0
title: CreateDirectory
permalink: /docs/5-0/operations/remote/create-directory/
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
public class MyRemoteOperation : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.CreateDirectory(@"C:/temp/MyDir"));
    }
}
{% endhighlight %}
