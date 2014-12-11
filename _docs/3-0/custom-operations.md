---
layout: doc
title: Custom Operations
permalink: /docs/3-0/custom-operations/
---

Custom Operations
=================

There are three types of Custom Operations in ConDep:

* Local Operation
* Remote Composite Operation
* Remote Server Operation

First we want to describe each operation type and their differences, and then we will go into the details of how you would implement each of these later in this section.

## Local Operation

A **Local Operation** is a operation that executes locally. Or more accurately, a operation that is executed on the ConDep host (where ConDep.exe is executed) and not on any of the defined target servers in your [environments](../environment/). This is more easily explained by two examples:

Example 1:

> The [`TransformConfigFile`](https://github.com/condep/condep-dsl-operations/blob/master/src/ConDep.Dsl.Operations/Local/TransformConfig/TransformConfigOperation.cs) is a **Local Operation** that transforms .NET configuration files. It is **Local** because it runs on the ConDep host, and transform .NET configurations files located on that host. This is quite simple and logical.

Example 2:

> The [`HttpGetOperation`](https://github.com/condep/condep-dsl-operations/blob/master/src/ConDep.Dsl.Operations/Local/WebRequest/HttpGetOperation.cs) is also a **Local Operation**, that execute HTTP GET operations to defined URL's. But how can this be a **Local Operation** when it execute remote URI's? That the operation calls remote URI's using HTTP GET is a implementation detail for ConDep. The only thing that matters for ConDep is where it is executed; which is locally on the ConDep host, as oppose to on any of your servers in your defined environments.

Hopefully these two examples have helped you in understanding what makes a **Local Operation** different from a **Remote Operation** (explained below). If not, reading about the two other **Remote** operations will hopefully make it more clear.

## Remote Composite Operation

A **Remote Composite Operation** is a operation composed of one or several other remote operations (**Composite** or **Server** operations). Creating a **Remote Composite Operation** is very much like creating a [Artifact](../artifact/), since you get access to the ConDep DSL and can *compose* a new operation by using existing operations.

## Remote Server Operation

A **Remote Server Operation** on the other hand is code that will be executed on the target server. You can write normal C# code and ConDep will take care of the complexity of getting this code to execute on the servers in your [environments](../environment/). 