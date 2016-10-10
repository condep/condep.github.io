---
layout: doc-4-0
title: About Custom Operations
permalink: /docs/4-0/custom-operations/
---

The ConDep DSL have solved the difficult problems with orchestration, deployment,
installation and remote execution on Windows. That is however not always enough.

Typically there are two main reasons for creating your own custom operation:

1. The functionality you need is not currently offered in the existing ConDep
DSL
2. You want to group a sequence of operations which you want to reuse elsewhere,
instead of using a separate Artifact.

Examples of *sequence of operations* could be the operations needed to
install and configure a application on your servers (like a Build Agent, ADFS or
IIS). These applications typically requires more than a install command and
needs additional configuration. Another example could be company specific
requirements you want to group into one operation.

Often you start out with an Artifact and later see that all or parts of the
operations you use in your Artifact can be added to a custom operation, allowing
you (or others) to reuse the operation elsewhere.

<div class="note info">
    <h2>You will probably want to use the Remote Composite Operation</h2>
    <p>
        Unless you're very familiar with ConDep you will probably want to start
        with a <code>RemoteCompositeOperation</code>. This should take care of
        90-100% of your needs. If you find that it does not solve your problem,
        come back here and read about the other operation types.
    </p>
</div>

<div class="note info">
    <h2>
        Consider contributing to the ConDep community
    </h2>
    <p>
        When you create a custom operation and you realize this would benefit others
        in the ConDep community, we encourage you to contribute these back
        so others could make use of your great ideas. The ConDep repository
        <a href="https://github.com/condep/condep-dsl-operations-contrib">
            <i>condep-dsl-operations-contrib</i>
        </a> is specifically created for this purpose. And if your lucky your
        custom operation might be so useful that we decide to add it to the
        core ConDep DSL.
    </p>
</div>

## Custom Operation Types

There are four types of operations in ConDep:

* [Remote Composite Operation (`RemoteCompositeOperation`)](#remote-composite-operation)
* [Local Operation (`LocalOperation`)](#local-operation)
* [For Each Server Operation (`ForEachServerOperation`)](#for-each-server-operation)
* [Remote Code Operation (`RemoteServerOperation`)](#remote-code-operation)

Below we describe each operation type and how they relate to the other
operations, but if you want to jump ahead and see example of implementation see:

* [Implementing a Local Operation](../local-operation/)
* [Implementing a For Each Server Operation](../for-each-server-operation/)
* [Implementing a Remote Composite Operation](../remote-composite-operation)
* [Implementing a Remote Code Operation](../remote-server-operation/)

<a name="local-operation"></a>

## Remote Composite Operation

Allows you to author a new Operation by combining existing remote Operations. A
**Remote Composite Operation** gives you access to ConDep's DSL for remote
Operations (just like Artifacts). Local Operations will not be available.

The most common usage is to use the remote PowerShell Operation available in the
remote DSL to execute your custom script, typically combined with deploy
operations.

This is the most common and easiest custom Operation to author in ConDep.

See [Implementing a Remote Composite Operation](../remote-composite-operation)
for more details.

## Local Operation

Runs on the same machine that runs `condep.exe` rather than logging in to a
remote server to run the operation. This also means it only runs once and
not for every server in your Environment(/Tier).

Here's two **Local Operations** in [ConDep.Dsl.Operations](https://github.com/condep/condep-dsl-operations)
that illustrate two quite different **Local Operations**:

Example 1:

> The [`TransformConfigFile`](https://github.com/condep/condep-dsl-operations/blob/master/src/ConDep.Dsl.Operations/Local/TransformConfig/TransformConfigOperation.cs)
> is a **Local Operation** that transforms .NET configuration files. It is **Local**
> because it runs on the ConDep host, and transform .NET configurations files located
> on that host. This is quite simple and logical.

Example 2:

> The [`HttpGetOperation`](https://github.com/condep/condep-dsl-operations/blob/master/src/ConDep.Dsl.Operations/Local/WebRequest/HttpGetOperation.cs)
> is also a **Local Operation**, that execute HTTP GET operations to defined URL's.
> But how can this be a **Local Operation** when it execute remote URI's? That the
> operation calls remote URI's using HTTP GET is a implementation detail for ConDep.
> The only thing that matters for ConDep is where it is executed; which is locally on
> the ConDep host, as oppose to on any of your servers in your defined environments.

See [Implementing a Local Operation](../local-operation/) for more details.

<a name="for-each-server-operation"></a>

## For Each Server Operation

A **For Each Server Operation** is (as its name implies) executed once for each
server in your Environment(/Tier). Typical usage is when you need to use some kind of
third party API to communicate with the server.

Examples of existing For Each Server Operations in [ConDep.Dsl.Operations](https://github.com/condep/condep-dsl-operations)
are: `CopyFileOperation`, `CopyDirOperation` and `RestartComputerOperation`.

See [Implementing a For Each Server Operation](../for-each-server-operation/) for
more details.

<a name="remote-code-operation"></a>

## Remote Code Operation

A **Remote Code Operation** contains .NET code that will be deployed by ConDep
to the servers you have defined in your Environment and executed locally on
those servers. You will typically only need this operation type if you want to
execute C# code on the server, instead of using PowerShell. Some things are
still easier done in pure C# (like using .NET classes or calling into the
WIN32 API), and this is when you reach for the **Remote Code Operation**.

See [Implementing a Remote Code Operation](../remote-server-operation/) for
more details.

<a name="remote-composite-operation"></a>
