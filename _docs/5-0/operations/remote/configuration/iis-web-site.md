---
layout: doc-5-0
title: IISWebSite
permalink: /docs/5-0/operations/remote/configuration/iis-web-site/
version_added: 3.1.0
---

{% highlight csharp %}
public class MyRemoteConfiguration : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.Configure.IISWebSite("my_web_site", 101));
    }
}
{% endhighlight %}
