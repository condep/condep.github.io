---
layout: doc-5-0
title: Implementing a Remote Code Operation
prev_section: implement-remote-operation
next_section: custom-loadbalancer
permalink: /docs/5-0/implement-remote-code-operation/
---

Here we show how you would implement a **Remote Code Operation** (
`RemoteCodeOperation` class) in ConDep. For information on what a
**Remote Server Operation** is and how it relates to the other operation
types in ConDep, go to
[Custom Operations](../custom-operations/#remote-code-operation).

Say you want to make an operation that adds a user to a group on a remote server. The operation for doing that, would look like this.

{% highlight csharp %}
public class AddUserToGroupOperation : RemoteCodeOperation
{
    private readonly string _userName;
    private readonly string _groupName;

    public override string Name => "Add " + _userName +" to " + _groupName;

    public AddUserToGroupOperation(string userName, string groupName) : base(userName, groupName)
    {
        _userName = userName;
        _groupName = groupName;
    }

    public override void Execute(ILogForConDep logger)
    {
        try
        {
            var group = new DirectoryEntry("WinNT://./" + _groupName);
            logger.Info($"Adding user {_userName} to group {group.Name}");

            group.Invoke("Add", "WinNT://" + _userName.Replace("\\", "/"));
            logger.Info("User added");
        }
        catch (Exception ex)
        {
            if (ex.InnerException == null || !ex.InnerException.Message.Contains("account name is already a member"))
                logger.Warn("Failed to add user to group!");
        }
    }
}
{% endhighlight %}

You need to pass the variables to tha base class, that is the clue here.

To make this operation available in the ConDep DSL, you create an extension method:

{% highlight csharp %}
public static void AddUserToGroup(this IOfferRemoteConfiguration configuration, string userName, string groupName)
{
    var operation = new AddUserToGroupOperation(userName, groupName);
    OperationExecutor.Execute((RemoteBuilder)configuration, operation);
}
{% endhighlight %}

And you can use it anywhere (like from an Runbook) like this:

{% highlight csharp %}
public class SomeRunbookAddingUsersToGroups : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.Configure.AddUserToGroup("your_username", "Administrators");
    }
}
{% endhighlight %}

## The different kinds of remote operations.
In the runbook example above, you can see that the DSL use `remote.Configure`, and looks like this:

`dsl.Remote(remote => remote.Configure.AddUserToGroup(...)` 

The reason it's using Configure is because of the first variable in the extension class. In the example above, you can see that IOfferRemoteConfiguration is used.

If you are building other operations then configuration operations you can instead use another when you are creating your extension class:

* IOfferRemoteOperations -> dsl.Remote(remote => remote.MyCustomOperation(...))
* IOfferRemoteConfiguration -> dsl.Remote(remote => remote.Configure.MyCustomOperation(...))
* IOfferRemoteInstallation -> dsl.Remote(remote => remote.Install.MyCustomOperation(...))
* IOfferRemoteDeployment -> dsl.Remote(remote => remote.Deploy.MyCustomOperation(...))
* IOfferRemoteExecution -> dsl.Remote(remote => remote.Execute.MyCustomOperation(...))
