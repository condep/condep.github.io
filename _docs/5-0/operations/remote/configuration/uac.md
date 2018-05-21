---
layout: doc-5-0
title: UserAccountControl
permalink: /docs/5-0/operations/remote/configuration/uac/
version_added: 3.2.0
---

Turn User Account Control (UAC) off or on.

## Syntax

{% highlight csharp %}
UserAccountControl(bool enabled)
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
      <td>enabled</td>
      <td>Set to False to disable UAC, True to enable. By default UAC is enabled on Windows Servers.</td>
    </tr>
	</tbody>
</table>

## Code Example

{% highlight csharp %}
public class MyRemoteConfiguration : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.Configure.UserAccountControl(true));
    }
}
{% endhighlight %}
