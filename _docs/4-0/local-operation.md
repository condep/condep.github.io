---
layout: doc-4-0
title: Implementing a Local Operation
permalink: /docs/4-0/local-operation/
---

Here we show how you would implement a **Local Operation** in ConDep. For
information on what a **Local Operation** is and how it relates to the other
operation types in ConDep, go to [Custom Operations](../custom-operations/#
local-operation).

Say we wanted to have a **Local Operation** that send a Text Message (SMS), we
could define it like this:

{% highlight csharp %}
public class SmsOperation : LocalOperation
{
    private readonly string _phoneNumber;
    private readonly string _message;

    public SmsOperation(string phoneNumber, string message)
    {
        _phoneNumber = phoneNumber;
        _message = message;
    }

    public override void Execute(IReportStatus status, ConDepSettings settings, CancellationToken token)
    {
        var sms = new SmsLibrary.Sms(_phoneNumber);
        sms.Send(_message);
    }

    public override bool IsValid(Notification notification)
    {
        return _message.Length <= 160;
    }

    public override string Name
    {
        get { return "Sms"; }
    }
}
{% endhighlight %}

To make this operation available in the ConDep DSL, you create an extension method:

{% highlight csharp %}
public static class SmsExtensions
{
    public static void Sms(this ConDep.Dsl.IOfferLocalOperations local, string phoneNumber, string message)
    {
        var op = new SmsOperation(phoneNumber, message);
        Configure.Operation(local, op);
    }
}
{% endhighlight %}

And you can use it anywhere (like from an Artifact) like this:

{% highlight csharp %}
public class SomeArtifactUsingSms : Artifact.Local
{
    public override void Configure(IOfferLocalOperations onLocalMachine, ConDepSettings settings)
    {
        onLocalMachine.Sms("+47 99999999", "Hello from ConDep!!");
    }
}
{% endhighlight %}
