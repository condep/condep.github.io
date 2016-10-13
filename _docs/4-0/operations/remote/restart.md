---
layout: doc
title: Restart
permalink: /3-0/remote/restart/
version_added: 3.1.0
---

Restart server. This operation will wait until the server comes back online and ConDep can successfully communicate with it again.

## Syntax

{% highlight csharp %}
Restart()
{% endhighlight %}

{% highlight csharp %}
Restart(int delayInSeconds)
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
			<td>delayInSeconds (optional)</td>
			<td>How many seconds to wait before issue the restart operation on the server. Default is 10 seconds.</td>
		</tr>
	</tbody>
</table>

## Code Example

{% highlight csharp %}
public class MyRemoteArtifact : Artifact.Remote
{
  public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
  {
    server.Restart();
  }
}
{% endhighlight %}
