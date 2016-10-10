---
layout: doc-4-0
title: Environments
prev_section: push-based
next_section: artifacts
permalink: /docs/4-0/environment/
---

Environment files in ConDep is where you put your environment specific configurations.
Are the web servers in your test environment named differently from production? Is the
server names the same, but FQDN difers? Different Load Balancer? Different admin
credentials (I would hope so!)?

If you have three environments you commonly refer to as Dev, Test and Prod - you would
create three environment configuration files:

* `dev.env.json`
* `test.env.json`
* `prod.env.json`

Here´s a environment file (`prod.env.json`) example:

{% highlight json %}
{
  "Servers" :
  [
    {
      "Name" : "web01.prod.local"
    },
    {
      "Name" : "web02.prod.local"
    },
    {
      "Name" : "web03.prod.local"
    }
  ],
  "DeploymentUser":
  {
    "UserName": "Administrator",
    "Password": "ReallySecureP@$$w0rd :-)"
  }
}
{% endhighlight %}

`condep.exe` takes several mandatory parameters, one which is the environment name. Note
that you only use the name, excluding .env.json.

## Tiers

To differentiate between environments is great, but what about Tiers? You probably have
servers in your environments which you can categorize into different Tiers. Common Tiers
would be a Web-Tier, an App-Tier and a Data-Tier. It's up to you what you call them and
how many you have.

Here's a environment file (`prod.env.json`) with Tiers example:

{% highlight json %}
{
  "Tiers":
  [
    {
      "Name" : "Web",
      "Servers" :
      [
        {
          "Name" : "web01.prod.local"
        },
        {
          "Name" : "web02.prod.local"
        }
      ]
    },
    {
      "Name" : "App",
      "Servers" :
      [
        {
          "Name" : "app01.prod.local"
        },
        {
          "Name" : "app02.prod.local"
        }
      ]
    },
    {
      "Name" : "Data",
      "Servers" :
      [
        {
          "Name" : "data01.prod.local"
        },
        {
          "Name" : "data02.prod.local"
        }
      ]
    }
  ],
  "DeploymentUser":
  {
    "UserName": "Administrator",
    "Password": "ReallySecureP@$$w0rd :-)"
  }
}
{% endhighlight %}

You don't pass the tier to `condep.exe` as you do with the environment name, you
use a .NET attribute on you Artifact instead. You can find out more about how
you do this under [Artifacts](../artifacts/).
