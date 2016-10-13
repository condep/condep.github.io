---
layout: doc
title: UserAccountControl
permalink: /3-0/infrastructure/uac/
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
public class MyRemoteArtifact : Artifact.Remote
{
  public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
  {
    server.UserAccountControl(enabled: false);
  }
}
{% endhighlight %}
