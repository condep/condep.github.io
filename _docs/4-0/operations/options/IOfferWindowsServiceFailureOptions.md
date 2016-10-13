---
layout: doc
title: IOfferWindowsServiceFailureOptions
permalink: /3-0/options/IOfferWindowsServiceFailureOptions/
version_added: 3.1.0
---

Sets failure options for a Windows Service. You can specify failure options for first, second and subsequent failures using the below properties:

<table>
	<thead>
		<tr>
			<th>Property</th>
			<th>Description</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>
{% highlight csharp %}
FirstFailure
{% endhighlight %}
			</td>
			<td>
Provide possible actions that should be performed after first failure of a Windows Service.
			</td>
		</tr>
		<tr>
			<td>
{% highlight csharp %}
SecondFailure
{% endhighlight %}
			</td>
			<td>
Provide possible actions that should be performed after second failure of a Windows Service.
			</td>
		</tr>
		<tr>
			<td>
{% highlight csharp %}
SubsequentFailures
{% endhighlight %}
			</td>
			<td>
Provide possible actions that should be performed after every subsequent Windows Service failure (after first and second).
			</td>
		</tr>
	</tbody>
</table>

And here are the possible options per of the above properties:

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
.TakeNoAction()
{% endhighlight %}
			</td>
			<td>
Will take no action on service failure
			</td>
		</tr>
		<tr>
			<td>
{% highlight csharp %}
.RestartService(int delayInMilliseconds)
{% endhighlight %}
			</td>
			<td>
Will restart the Windows Service after given amount of milliseconds on service failure.
			</td>
		</tr>
		<tr>
			<td>
{% highlight csharp %}
.RunProgram(
	string program,
	string parameters,
	int delayInMilliseconds
)
{% endhighlight %}
			</td>
			<td>
Will run a program with given parameters after the defined amount of milliseconds on service failure.
			</td>
		</tr>
		<tr>
			<td>
{% highlight csharp %}
.RestartComputer(int delayInMilliseconds)
{% endhighlight %}
			</td>
			<td>
Will restart computer after given amount of millisecond on service failure.
			</td>
		</tr>
	</tbody>
</table>
