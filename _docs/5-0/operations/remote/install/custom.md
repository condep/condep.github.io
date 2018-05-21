---
layout: doc-5-0
title: Custom
permalink: /docs/5-0/operations/remote/install/custom/
version_added: 3.1.0
---

{% highlight csharp %}
public class MyRemoteInstallation : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.Install.Custom("package_name", "exe_path", "exe_params"));
    }
}
{% endhighlight %}
