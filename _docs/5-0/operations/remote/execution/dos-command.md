---
layout: doc-5-0
title: DosCommand
permalink: /docs/5-0/operations/remote/execution/dos-command
version_added: 3.1.0
---

Will execute a DOS command using cmd.exe on remote server. ConDep will check the exit code and throw if exit code is different from 0.

## Syntax

{% highlight csharp %}
DosCommand(string cmd)
{% endhighlight %}

{% highlight csharp %}
DosCommand(string cmd, Action<IOfferRunCmdOptions> runCmdOptions)
{% endhighlight %}

<table>
	<thead>
		<tr>
			<th>Parameter</th>
			<th>Description</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>cmd</td>
			<td>The command to execute</td>
		</tr>
		<tr>
			<td>runCmdOptions</td>
			<td>See below</td>
		</tr>
	</tbody>
</table>

##IOfferRunCmdOptions

<table>
	<thead>
		<tr>
			<th>Function</th>
			<th>Description</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>
{% highlight csharp %}
ContinueOnError(bool continueOnError)
{% endhighlight %}
			</td>
			<td>
				Will continue execution ignoring any exit code
			</td>
		</tr>
	</tbody>
</table>

## Code Example

{% highlight csharp %}
public class MyRemoteOperation : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.Execute.DosCommand("ipconfig"));
    }
}
{% endhighlight %}
