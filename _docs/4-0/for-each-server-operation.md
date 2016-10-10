---
layout: doc-4-0
title: Implementing a For Each Server Operation
permalink: /docs/4-0/for-each-server-operation/
---

Here we show how you would implement a **For Each Server Operation** in
ConDep. For information on what a **For Each Server Operation** is and how
it relates to the other operation types in ConDep, go to
[Custom Operations](../custom-operations/#for-each-server-operation).

Say we wanted to have a **For Each Server Operation** that ping servers to see
if they respond, we could define it like this:

{% highlight csharp %}
public class PingOperation : ForEachServerOperation
{
    public override void Execute(ServerConfig server, IReportStatus status, ConDepSettings settings, CancellationToken token)
    {
        var ping = new Ping();
        var reply = ping.Send(server.Name);

        if(reply.Status != IPStatus.Success) throw new PingException("Failed to ping server");
    }

    public override string Name
    {
        get { return "Ping Operation"; }
    }

    public override bool IsValid(Notification notification)
    {
        return true;
    }
}
{% endhighlight %}

To make this operation available in the ConDep DSL, you create an extension method:

{% highlight csharp %}
public static class PingExtensions
{
    public static void Ping(this ConDep.Dsl.IOfferRemoteOperations remote)
    {
        var op = new PingOperation();
        Configure.Operation(remote, op);
    }
}
{% endhighlight %}

And you can use it anywhere (like from an Artifact) like this:

{% highlight csharp %}
public class SomeArtifactUsingPing : Artifact.Remote
{
    public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
    {
        server.Ping();
    }
}
{% endhighlight %}
