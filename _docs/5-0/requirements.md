---
layout: doc-5-0
title: System Requirements
prev_section: new
next_section: terms
permalink: /docs/5-0/requirements/
---

ConDep will validate client and server PowerShell versions, and that it can remotely
communicate with the configured servers before doing any actual work on them. It will
**not**, however, validate operating system versions as defined in the General
requirements below. Some [Operations](../condep-dsl-operations/) might work on OS
versions lower than the recommended, but not all.

Windows Server 2012 and Windows 8 are enabled for Windows PowerShell Remoting by default.
If the settings are changed, you can restore the default settings by by running the
`Enable-PSRemoting` cmdlet.

On all other supported versions of Windows, you need to run the `Enable-PSRemoting` cmdlet
to enable Windows PowerShell Remoting.

## General requirements
<table>
	<thead>
		<tr>
			<th></th><th>Client</th><th>Server</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>Operating System</td><td>&gt;= Windows 7 SP1 / Windows Server 2008 R2</td><td>&gt;= Windows Server 2008 R2</td>
		</tr>
		<tr>
			<td>PowerShell Version</td><td>&gt;= 3.0</td><td>&gt;= 3.0</td>
		</tr>
		<tr>
			<td>.NET Framework</td><td>&gt;= 4.0</td><td>&gt;= 4.0</td>
		</tr>
	</tbody>
</table>

In addition, Windows Remote Management (WinRM) must be enabled on both client
and servers. You can find more information about enabling WinRM from this Microsoft
article: [http://support.microsoft.com/kb/555966](http://support.microsoft.com/kb/555966)

## Firewalls and Ports
From Microsoft ( [http://msdn.microsoft.com/en-us/library/aa384372(v=vs.85).aspx](http://msdn.microsoft.com/en-us/library/aa384372(v=vs.85) ):

> Starting in WinRM 2.0, the default listener ports configured by `winrm
> quickconfig` are port 5985 for HTTP transport and port 5986 for HTTPS.
> WinRM listeners can be configured on any arbitrary port.


> If a computer is upgraded to WinRM 2.0, the previously configured
> listeners are migrated and still receive traffic.

The ConDep [Node](../security/#node) by default listens on TCP port 4444 using SSL.

## Credentials

To create remote sessions and run remote commands, by default, the current
user must be a member of the **Administrators** group on the remote computer or
provide the credentials of an **Administrator**. Otherwise, the command will fail.
