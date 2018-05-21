---
layout: doc-5-0
title: Restart
permalink: /docs/5-0/operations/remote/restart/
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
public class MyRemoteOperation : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.Restart());
    }
}
{% endhighlight %}
