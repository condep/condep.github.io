---
layout: page
title: System Requirements
prev_section: new
next_section: security
permalink: /docs/requirements/
---

System Requirements
==========================
ConDep will validate client and server PowerShell versions and that it can remotely communicate with the configured servers before doing any actual work on them. It will however not validate operating system versions as defined in the General requirements below. Some operations might work on OS versions lower than defined, but not all.

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

In addition, Windows Remote Management (WinRM) must be enable on client and servers. You can find more information about enabling WinRM from this Microsoft article: [http://support.microsoft.com/kb/555966](http://support.microsoft.com/kb/555966) 

## Credentials


## Default WinRM settings server Operating Systems
<table>
	<thead>
		<tr>
			<td></td><td>WinRM</td><td>Server</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>Windows Server 2012 R2</td><td>Enabled</td><td>&gt;= Windows Server 2008 R2</td>
		</tr>
		<tr>
			<td>Windows Server 2012</td><td>Enabled</td><td>&gt;= 3.0</td>
		</tr>
		<tr>
			<td>Windows Server 2008 R2</td><td>&gt;= 4.0</td><td>&gt;= 4.0</td>
		</tr> 	
	</tbody>
</table>

## Default WinRM settings client Operating Systems
<table>
	<thead>
		<tr>
			<td></td><td>WinRM</td><td>Server</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>Windows 8.1</td><td>&gt;= Windows 7 / Windows Server 2008 R2</td><td>&gt;= Windows Server 2008 R2</td>
		</tr>
		<tr>
			<td>Windows 8</td><td>&gt;= 3.0</td><td>&gt;= 3.0</td>
		</tr>
		<tr>
			<td>Windows 7 SP1</td><td>&gt;= 4.0</td><td>&gt;= 4.0</td>
		</tr> 	
	</tbody>
</table>
