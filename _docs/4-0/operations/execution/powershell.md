---
layout: doc
title: PowerShell
permalink: /3-0/execution/powershell/
version_added: 3.1.0
---

Will execute a DOS command using cmd.exe on remote server. ConDep will check the exit code and throw if exit code is different from 0.

## Syntax

Execute a simple PowerShell command

{% highlight csharp %}
PowerShell(string command)
{% endhighlight %}

Execute a PowerShell script from file (.ps1)

{% highlight csharp %}
PowerShell(FileInfo scriptFile)
{% endhighlight %}

Execute a simple PowerShell command with additional options
{% highlight csharp %}
PowerShell(string command, Action<IOfferPowerShellOptions> powerShellOptions)
{% endhighlight %}

Execute a PowerShell script from file (.ps1) with simple options

{% highlight csharp %}
PowerShell(FileInfo scriptFile, Action<IOfferPowerShellOptions> powerShellOptions)
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
			<td>command</td>
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
public class MyRemoteArtifact : Artifact.Remote
{
  public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
  {
    server.Execute.DosCommand("ipconfig");
  }
}
{% endhighlight %}
