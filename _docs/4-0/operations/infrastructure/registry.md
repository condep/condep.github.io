---
layout: doc
title: WindowsRegistry
permalink: /3-0/infrastructure/registry/
version_added: 3.2.0
---

Adds or removes elements from the Windows Registry.

## Syntax

{% highlight csharp %}
WindowsRegistry(Action<IOfferWindowsRegistryOperations> reg)
{% endhighlight %}

The WindowsRegistry operation offers a lamda of Registry operations:

* CreateKey
* DeleteKey
* SetValue
* DeleteValue

## CreateKey

In its basic form this operation will just create a Windows Registry key. However, if you use the available options lamda, you can continue with values and sub-keys - allowing you to create the entire registry tree you need.

{% highlight csharp %}
CreateKey(WindowsRegistryRoot root, string key)
{% endhighlight %}

{% highlight csharp %}
CreateKey(WindowsRegistryRoot root, string key, Action<IOfferWindowsRegistryOptions> options)
{% endhighlight %}

{% highlight csharp %}
CreateKey(WindowsRegistryRoot root, string key, string defaultValue)
{% endhighlight %}

{% highlight csharp %}
CreateKey(WindowsRegistryRoot root, string key, string defaultValue, Action<IOfferWindowsRegistryOptions> options)
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
      <td>root</td>
      <td>Which root in the registry? <code>WindowsRegistryRoot</code> allow you to specify these hives:
        <ul>
          <li>HKEY_LOCAL_MACHINE</li>
          <li>HKEY_CURRENT_USER</li>
          <li>HKEY_USERS</li>
          <li>HKEY_CLASSES_ROOT</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>key</td>
      <td>Name of the Windows Registry Key to create (e.g. SOFTWARE\ConDep)</td>
    </tr>
    <tr>
      <td>defaultValue</td>
      <td>The default value for the Windows Registry Key. This is most commonly left blank.</td>
    </tr>
    <tr>
      <td>options</td>
      <td>Using options found in <code>IOfferWindowsRegistryOptions</code> you can add <code>Values</code> and <code>SubKeys</code>.</td>
    </tr>
	</tbody>
</table>

* Creates a new Key with no default value.
* Creates a new Key with additional options (like values and sub-keys).
* Creates a new Key with a default value.
* Creates a new Key with a default value and additional options (like values and sub-keys).

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
