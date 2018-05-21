---
layout: doc-5-0
title: Msi
permalink: /docs/5-0/operations/remote/install/msi/
version_added: 3.1.0
---

{% highlight csharp %}
public class MyRemoteInstallation : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.Install.Msi("package_name", "msi_path"));
    }
}
{% endhighlight %}
