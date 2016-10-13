---
layout: doc
title: IOfferWindowsServiceOptions
permalink: /3-0/options/IOfferWindowsServiceOptions/
version_added: 3.1.0
---

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
UserName(string username)
{% endhighlight %}
			</td>
			<td>
				To run the Windows Service under a custom account, provide <code>domain\user</code> or <code>.\user</code> for local user. For built-in accounts like NetworkService, use <code>.\NetworkService</code>. If no user is provided, <code>LocalSystem</code> will be used.
			</td>
		</tr>
		<tr>
			<td>
{% highlight csharp %}
Password(string password)
{% endhighlight %}
			</td>
			<td>
Password for the custom account if UserName is set.
			</td>
		</tr>
		<tr>
			<td>
{% highlight csharp %}
Description(string description)
{% endhighlight %}
			</td>
			<td>
A description for the Windows Service.
			</td>
		</tr>
		<tr>
			<td>
{% highlight csharp %}
ServiceGroup(string group)
{% endhighlight %}
			</td>
			<td>
A service group for the Windows Service. Windows Services within the same Service Group and auto start set, will start together when a computer starts up. There are also other reasons for using Service Groups.
			</td>
		</tr>
		<tr>
			<td>
{% highlight csharp %}
ExeParams(string parameters)
{% endhighlight %}
			</td>
			<td>
Provide any parameters you want to be sent in to the Windows Service executable on startup.
			</td>
		</tr>
		<tr>
			<td>
{% highlight csharp %}
IgnoreFailureOnServiceStartStop(bool value)
{% endhighlight %}
			</td>
			<td>
During installation or removal, will ignore any errors during start/stop of the Windows Service.
			</td>
		</tr>
		<tr>
			<td>
{% highlight csharp %}
StartupType(ServiceStartMode type)
{% endhighlight %}
			</td>
			<td>
Defined the startup type for the Windows Service.
			</td>
		</tr>
		<tr>
			<td>
{% highlight csharp %}
DoNotStartAfterInstall()
{% endhighlight %}
			</td>
			<td>
If used, will not start the Windows Service after installation.
			</td>
		</tr>
		<tr>
			<td>
{% highlight csharp %}
OnServiceFailure(
  int serviceFailureResetInterval,
  Action<IOfferWindowsServiceFailureOptions> options
)
{% endhighlight %}
			</td>
			<td>
Defines actions for first, second and subsequent failures of the Windows Service. See <a href="../IOfferWindowsServiceFailureOptions/">IOfferWindowsServiceFailureOptions</a> for details.
			</td>
		</tr>
		<tr>
			<td>
{% highlight csharp %}
TimeoutInSeconds(int timeout)
{% endhighlight %}
			</td>
			<td>
Timeout for how long ConDep will wait on start or stop for windows service
			</td>
		</tr>
	</tbody>
</table>
