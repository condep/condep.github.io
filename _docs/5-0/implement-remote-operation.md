---
layout: doc-5-0
title: Implementing a Remote Operation
prev_section: implement-local-operation
next_section: implement-remote-code-operation
permalink: /docs/5-0/implement-remote-operation/
---

Here we show how you would implement a **Remote Operation**
in ConDep. For information on what a **Remote Operation** is
and how it relates to the other operation types in ConDep, go to
[Custom Operations](../custom-operations/#remote-operation).

Say you want to make an operation that sets the IIS Machine key on a remote server. We could define it like this:

{% highlight csharp %}
public class ConfigureIisMachineKeyOperation : RemoteOperation
{
    private readonly string _validationKey;
    private readonly string _decryptionKey;
    private readonly string _validation;
    private readonly string _decryption;

    public ConfigureIisMachineKeyOperation(string validationKey, string decryptionKey, string validation, string decryption)
    {
        _validationKey = validationKey;
        _decryptionKey = decryptionKey;
        _validation = validation;
        _decryption = decryption;
    }

    public override Result Execute(IOfferRemoteOperations remote, ServerConfig server, ConDepSettings settings, CancellationToken token)
    {
        remote.Execute.PowerShell($"Set-MachineKeys -validationKey { _validationKey } -decryptionKey { _decryptionKey }  -validation { _validation } -decryption {_decryption}");
        return new Result(true, false);
    }

    public override string Name => "Configure IIS Machine Key";
}
{% endhighlight %}

To make this operation available in the ConDep DSL, you create an extension method:

{% highlight csharp %}
public static void IisMachineKey(this IOfferRemoteConfiguration configuration, string validationKey, string decryptionKey, string validation, string decryption = "auto")
{
    var operation = new ConfigureIisMachineKeyOperation(validationKey, decryptionKey, validation, decryption);
    OperationExecutor.Execute((RemoteBuilder)configuration, operation);
}
{% endhighlight %}

And you can use it anywhere (like from an Runbook) like this:

{% highlight csharp %}
public class SomeRunbookThatSetsTheIisMachineKey : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.Configure.IisMachineKey("valKey", "decrKey", "validation"));
    }
}
{% endhighlight %}

## The different kinds of remote operations.
In the runbook example above, you can see that the DSL use `remote.Configure`, and looks like this:

`dsl.Remote(remote => remote.Configure.IisMachineKey(...)` 

The reason it's using Configure is because of the first variable in the extension class. In the example above, you can see that IOfferRemoteConfiguration is used.

If you are building other operations then configuration operations you can instead use another when you are creating your extension class:

* IOfferRemoteOperations -> dsl.Remote(remote => remote.MyCustomOperation(...))
* IOfferRemoteConfiguration -> dsl.Remote(remote => remote.Configure.MyCustomOperation(...))
* IOfferRemoteInstallation -> dsl.Remote(remote => remote.Install.MyCustomOperation(...))
* IOfferRemoteDeployment -> dsl.Remote(remote => remote.Deploy.MyCustomOperation(...))
* IOfferRemoteExecution -> dsl.Remote(remote => remote.Execute.MyCustomOperation(...))