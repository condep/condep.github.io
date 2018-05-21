---
layout: doc-5-0
title: IISAppPool
permalink: /docs/5-0/operations/remote/configuration/iis-app-pool/
version_added: 3.1.0
---

{% highlight csharp %}
public class MyRemoteConfiguration : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.Configure.IISAppPool("my_app_pool"));
    }
}
{% endhighlight %}
