---
layout: doc-5-0
title: Implementing a Local Operation
prev_section: custom-operations
next_section: implement-remote-operation
permalink: /docs/5-0/implement-local-operation/
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

    public override Result Execute(ConDepSettings settings, CancellationToken token)
    {
        var sms = new SmsLibrary.Sms(_phoneNumber);
        sms.Send(_message);
        return new Result(success:true, changed:false);
    }

    public override string Name => "Send sms operation";
}
{% endhighlight %}

To make this operation available in the ConDep DSL, you create an extension method:

{% highlight csharp %}
public static class SmsExtensions
{
    public static void Sms(this IOfferLocalOperations local, string phoneNumber, string message)
    {
        var operation = new SmsOperation(phoneNumber, message);
        OperationExecutor.Execute((LocalBuilder)local, operation);
    }
}
{% endhighlight %}

And you can use it anywhere (like from an Runbook) like this:

{% highlight csharp %}
public class SomeRunbookUsingSms : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Local.Sms("+47 999 99 999", "Hello from ConDep!");
    }
}
{% endhighlight %}
