---
layout: doc-4-0
title: Environment Reference
permalink: /docs/4-0/env-reference/
---

<table>
  <thead>
    <tr>
      <th>Json Property</th><th>Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Servers</td>
      <td>
        In simplest form, you list your servers:

{% highlight json %}
{
  "Servers" :
  [
    {
      "Name" : "web01.prod.local"
    },
    {
      "Name" : "web02.prod.local"
    }
  ]
}
{% endhighlight %}
      </td>
    </tr>
    <tr>
      <td>DeploymentUser</td>
      <td>
The below user will be used for all Servers:

{% highlight json %}
{
  "DeploymentUser":
  {
    "UserName": "torresdal\\condepuser",
    "Password": "*********"
  }
}
{% endhighlight %}

You can define specific credentials per Server like this:

{% highlight json %}
{
  "Servers" :
  [
    {
      "Name" : "web01.prod.local",
      "DeploymentUser":
      {
        "UserName": "torresdal\\user1",
        "Password": "*********"
      }
    },
    {
      "Name" : "web02.prod.local",
      "DeploymentUser":
      {
        "UserName": "torresdal\\user2",
        "Password": "*********"
      }
    }
  ]
}
{% endhighlight %}

      </td>
    </tr>
    <tr>
      <td>LoadBalancer</td>
      <td>
{% highlight json %}
{
  "LoadBalancer":
  {
    "Name": "jat-nlb01",
    "Provider": "ConDep.Dsl.LoadBalancer.HaProxy.dll",
    "UserName": "torresdal\\nlbUser",
    "Password": "verySecureP@ssw0rd",
    "Mode": "Sticky"
  },
  "Servers" :
  [
    {
      "Name" : "web01.prod.local",
      "LoadBalancerFarm": "farm1"
    },
    {
      "Name" : "web02.prod.local",
      "LoadBalancerFarm": "farm1"
    }
  ]
}
{% endhighlight %}
      </td>
    </tr>
    <tr>
      <td>OperationsConfig</td>
      <td></td>
    </tr>
    <tr>
      <td>Tiers</td>
      <td></td>
    </tr>
    <tr>
      <td>PowerShellScriptFolders</td>
      <td></td>
    </tr>
  </tbody>
</table>
