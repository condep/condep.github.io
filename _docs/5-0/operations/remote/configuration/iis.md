---
layout: doc-5-0
title: Iis
permalink: /docs/5-0/operations/remote/configuration/iis/
version_added: 3.1.0
---

{% highlight csharp %}
public class MyRemoteConfiguration : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.Configure.IIS());
    }
}
{% endhighlight %}
