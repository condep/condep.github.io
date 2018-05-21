---
layout: doc-5-0
title: Windows
permalink: /docs/5-0/operations/remote/configuration/windows/
version_added: 3.1.0
---

{% highlight csharp %}
public class MyRemoteConfiguration : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.Configure.Windows(opt => opt.InstallFeature("feature_name")));
        dsl.Remote(remote => remote.Configure.Windows(opt => opt.UninstallFeature("feature_name")));
    }
}
{% endhighlight %}
