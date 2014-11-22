---
layout: doc
title: Quick Start
prev_section: security
next_section: push-based
permalink: /docs/3-0/quick-start/
---

Quick Start
===========
Coming soon...

{% highlight csharp %}
public class CredSSPTest : Artifact.Remote
{
    public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
    {
        server.Execute.PowerShell("write-host 'test credssp'", opt => opt.UseCredSSP(true));
    }
}
{% endhighlight %}
