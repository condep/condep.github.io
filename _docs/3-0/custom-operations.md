---
layout: doc
title: Custom Operations
permalink: /docs/3-0/custom-operations/
---

Custom Operations
=================

There are three types of Custom Operations in ConDep:

* Local Operation
* Remote Operation (Composite)
* Remote Operation (Server Code)

First we want to describe each operation type and their differences, and then we will go into the details of how you would implement each of these later in this section.

## Local Operation

A **Local Operation** is a operation that executes locally. Or more accurately, a operation that is executed on the ConDep host (where `ConDep.exe` is executed) and not on any of the defined target servers in your [environments](../environment/). Inherit from the `LocalOperation` class to create your own **Local Operation**. 

Here's two examples to illustrate two quite different **Local Operations**:

Example 1:

> The [`TransformConfigFile`](https://github.com/condep/condep-dsl-operations/blob/master/src/ConDep.Dsl.Operations/Local/TransformConfig/TransformConfigOperation.cs) is a **Local Operation** that transforms .NET configuration files. It is **Local** because it runs on the ConDep host, and transform .NET configurations files located on that host. This is quite simple and logical.

Example 2:

> The [`HttpGetOperation`](https://github.com/condep/condep-dsl-operations/blob/master/src/ConDep.Dsl.Operations/Local/WebRequest/HttpGetOperation.cs) is also a **Local Operation**, that execute HTTP GET operations to defined URL's. But how can this be a **Local Operation** when it execute remote URI's? That the operation calls remote URI's using HTTP GET is a implementation detail for ConDep. The only thing that matters for ConDep is where it is executed; which is locally on the ConDep host, as oppose to on any of your servers in your defined environments.

Hopefully these two examples have helped you understand what makes a **Local Operation** different from a **Remote Operation** (explained below). If not, reading about the two other **Remote** operations will hopefully make it more clear.

## Remote Operation (Composite)

The **Composite** version of the **Remote Operation** is a **Remote Operation** composed of one or several other **Remote Operations**. You inherit from `RemoteCompositeOperation` to create your own custom **Remote Operation**. 

Creating a **Composite** operation is very much like creating a [Artifact](../artifact/), since you get access to the ConDep DSL and can *compose* a new operation based of existing **Remote Operations**.

## Remote Operation (Server Code)

The **Server Code** version of the **Remote Operation** is code that will be executed on the target server. You can write normal C# code and ConDep will take care of the complexity of getting this code to execute on the servers in your [environments](../environment/), including all dependencies in referenced Assemblies. You inherit from `RemoteServerOperation` to create your own custom **Remote Operation**.

Most of the time you will want to use a **Composite** operation when you create custom operations in ConDep, but some things are still easier done in pure C# instead of for example PowerShell (like using .NET classes or calling into the WIN32 API), and this is when you reach for the **Server Code** operation. 

## Implementing a Local Operation

Say we wanted to have a **Local Operation** that send a Text Message (SMS), we could define it like this:

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


## Implementing a Remote Operation using Composition

## Implementing a Remote Operation using Server Code