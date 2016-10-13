---
layout: doc
title: AddUserToLocalGroup
permalink: /3-0/remote/add-user-to-group/
version_added: 3.2.0
---

Add local or domain user to local group.

## Syntax

{% highlight csharp %}
AddUserToLocalGroup(string userName, string groupName)
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
      <td>userName</td>
      <td>The user to add to group. If domain user, use the form domain\user.</td>
    </tr>
    <tr>
      <td>groupName</td>
      <td>The local group to add the user to.</td>
    </tr>
	</tbody>
</table>

## Code Example

{% highlight csharp %}
public class MyRemoteArtifact : Artifact.Remote
{
  public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
  {
    server.AddUserToLocalGroup("condep", "Administrators");
  }
}
{% endhighlight %}
