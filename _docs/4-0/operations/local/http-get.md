---
layout: doc
title: HttpGet
next_section: 3-0/operations/local/pre-compile
permalink: /3-0/local/http-get/
version_added: 3.1.0
---

Executes a simple HTTP GET to the specified url expecting a 200 (OK) in return.

<div class="note warning">
	<h2>HTTP Return Codes</h2>
  <p>
		This operation will throw an exception if return code is anything other than 200 (OK), including 301/302 redirects.
	</p>
</div>

<div class="note info">
  <p>
		A more advanced version of this operation is to be expected in future versions of ConDep.
	</p>
</div>

## Syntax

{% highlight csharp %}
HttpGet(string url)
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
			<td>url</td>
			<td>The URL to do a HTTP GET on</td>
		</tr>
	</tbody>
</table>

## Code Example

{% highlight csharp %}
public class MyLocalArtifact : Artifact.Local
{
	public override void Configure(IOfferLocalOperations onLocalMachine, ConDepConfig config)
	{
		onLocalMachine.HttpGet("http://www.google.com");
	}
}
{% endhighlight %}
