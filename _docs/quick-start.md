---
layout: page
title: Quick Start
permalink: /docs/quick-start/
---

Quick Start
===========
Some Text..

asdfasdf asdlfkjals df jalskj df
asldkfjas flkja sdlf

{% highlight csharp %}
public class CredSSPTest : Artifact.Remote
{
    public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
    {
        server.Execute.PowerShell("write-host 'test credssp'", opt => opt.UseCredSSP(true));
    }
}
{% endhighlight %}