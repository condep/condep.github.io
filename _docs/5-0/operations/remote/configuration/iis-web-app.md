---
layout: doc-5-0
title: IISWebApp
permalink: /docs/5-0/operations/remote/configuration/iis-web-app/
version_added: 3.1.0
---

{% highlight csharp %}
public class MyRemoteConfiguration : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.Configure.IISWebApp("my_web_app", "my_web_site"));
    }
}
{% endhighlight %}
