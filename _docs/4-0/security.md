---
layout: doc-4-0
title: Security
prev_section: requirements
next_section: quick-start
permalink: /docs/4-0/security/
---

Security in ConDep relates to two main topics:

* [Encrypting configuration files](#config)
* [Encrypting remote communication](#communication)

## <a name="config"></a>Configuration Encryption
Environment configuration files typically contains user names and passwords and
other sensitive data. To prevent this data to be added to source control or
visible to anyone that comes by the file, ConDep offers a simple command line
switch for encryption/decryption.

Before encryption:

{% highlight json %}
{
  "Servers" :
  [
    {
      "Name" : "ec2-54-216-139-21.eu-west-1.compute.amazonaws.com"
    }
  ],
  "DeploymentUser":
  {
    "UserName": "Administrator",
    "Password": "ReallySecureP@$$w0rd :-)"
  }
}
{% endhighlight %}

After encryption:

{% highlight json %}
{
  "Servers": [
    {
      "Name": "ec2-54-216-139-21.eu-west-1.compute.amazonaws.com"
    }
  ],
  "DeploymentUser": {
    "UserName": "Administrator",
    "Password": {
      "IV": "dtDhog4UEaTNqhgekeXNHg==",
      "Password": "I9n4/dwU8WFGa/mUOcgSO6/DhfZsIaRdyl85w/a/N10="
    }
  }
}
{% endhighlight %}

The example above uses structured data known to ConDep (DeploymentUser), but what
about unstructured data that is sensitive? For these cases you can encapsulate your
sensitive key/value inside `encrypt`, and it will be encrypted together with the
other data. Let's say you have an environment file with some json data like this:

{% highlight json %}
{
  "SomeKey": "MySensitiveValue",
  "SomeOtherKey": "NotSensitiveValue"
}
{% endhighlight %}

To encrypt `SomeKey`'s `MySensitiveValue`, just encapsulate it with `encrypt` like this:

{% highlight json %}
{
  "SomeKey":
  {
    "encrypt" : "MySensitiveValue"
  },
  "SomeOtherKey": "NotSensitiveValue"
}
{% endhighlight %}

When encrypted it would look like this:

{% highlight json %}
{
  "SomeKey": {
    "IV": "dtDhog4UEaTNqhgekeXNHg==",
    "SomeKey": "I9n4/dwU8WFGa/mUOcgSO6/DhfZsIaRdyl85w/a/N10="
  },
  "SomeOtherKey": "NotSensitiveValue"
}
{% endhighlight %}

## <a name="communication"></a>Remote communication

### PowerShell Remoting
ConDep use PowerShell Remoting for most of its work except transferring files
(which uses the ConDep Node). Underneath PowerShell Remoting is WinRM, a Web Service
for for remote interaction with Windows computers.

Before we go into encryption, lets just take a look at authentication. WinRM supports
6 authentication protocols:

* Basic
* Digest
* Kerberos (Default when client is in a Domain)
* Negotiate (either Kerberos or NTLM - Default when client is NOT in a Domain)
* Certificate
* CredSSP (See own section below)

For security reasons, ConDep only supports PowerShell's default authentication. Default
in PowerShell means **Kerberos** if both computers are within a Domain, or **Negotiate**
if not. **Negotate** will allow the two computers to settle on the strongest authentication
they can both agree on (Kerberos or NTLM version x). ConDep also support **CredSSP**,
which is covered in a separate section below.

So back to the reason for bringing up authentication.

<div class="note info">
	<h2>PowerShell encrypt HTTP traffic</h2>
  <p>
		As long as <b>Kerberos</b> or <b>Negotiate</b> is in use, PowerShell will encrypt
    the traffic even when using plain HTTP (no SSL).
	</p>
</div>

Since ConDep don't allow you to specify authentication type, traffic will always be encrypted.

<div class="note warning">
	<h2>When using ConDep outside a Windows Domain, we recommend using SSL</h2>
  <p>
	Even though traffic will be encrypted through PowerShell, the authenticity of the
    remote computer cannot be guarantied when both or one of the computers are NOT in a
    Windows Domain.
	</p>
</div>

To tell ConDep to use SSL, set `"SSL" : "true"` for the remote computer in the environment file:

{% highlight json %}
{
  "Servers" :
  [
    {
      "Name" : "ec2-54-216-139-21.eu-west-1.compute.amazonaws.com",
      "SSL" : "true"
    }
  ]
}
{% endhighlight %}

If you still decide against using SSL, you will have to add the remote computer to
WinRM's `TrustedHosts` list.

To do this using `winrm.exe` on your client:

{% highlight console %}
winrm s winrm/config/client '@{TrustedHosts="RemoteComputer#1, RemoteComputer#2, etc"}'
{% endhighlight %}

To do this using `PowerShell` on your client:

{% highlight powershell %}
Set-Item -Path WSMan:\localhost\Client\TrustedHosts -Value 'RemoteComputer#1, RemoteComputer#2, etc'
{% endhighlight %}

Alternatively, and really **NOT recommended**, you could trust **ALL**:
{% highlight console %}
winrm s winrm/config/client '@{TrustedHosts="*"}'
{% endhighlight %}

or

{% highlight powershell %}
Set-Item -Path WSMan:\localhost\Client\TrustedHosts -Value '*'
{% endhighlight %}

References:

* [WinRM (Windows Remote Management) Troubleshooting](http://blogs.technet.com/b/jonjor/archive/2009/01/09/winrm-windows-remote-management-troubleshooting.aspx) by JonJor
* [Monitoring with Windows Remote Management (WinRM) and Powershell Part I](https://www.myotherpcisacloud.com/post/2012/01/30/Monitoring-with-Windows-Remote-Management-(WinRM)-and-Powershell-Part-II.aspx) by Ryan Ries
* [Monitoring with Windows Remote Management (WinRM) and Powershell Part II](https://www.myotherpcisacloud.com/post/2012/01/26/Monitoring-with-Windows-Remote-Management-(WinRM)-and-Powershell-Part-I.aspx) by Ryan Ries


### CredSSP
[http://msdn.microsoft.com/en-us/library/ee309365(v=vs.85).aspx](Reference)

### <a name="node"></a>ConDep Node
ConDep Node is a self hosted HTTP Web API running as a Windows Service on all remote
servers ConDep communicate with. ConDep automatically deploys the Node the first time it
talks to a new server, and keeps it updated with the latest version used by the client.
The node (or service) does not run on the servers unless ConDep is executing remote
operations on it.

The node by default runs over SSL on port 4444. If you need to change the port you can
do this either from the command line when executing condep.exe (using `--nodePort=VALUE`)
or set it on individual servers in your `env.json` file. The server setting in `env.json`
has precedence over what you pass in on the command line, but ...
