---
layout: doc
title: PreCompile
permalink: /docs/5-0/operations/local/pre-compile
version_added: 3.1.0
---

Will pre-compile ASP.NET applications to prevent pre-compilation to occur when the application is first accessed. This will speed up startup times significantly on bigger ASP.NET Web Application.

<div class="note warning">
	<h2>Pre-Compile during build, not deploy!</h2>
  <p>
		We recommend you pre-compile web applications as part of your build process, not deployment process. This can be done using <a href="http://msdn.microsoft.com/en-us/library/ms229863(VS.100).aspx">aspnet_compiler.exe</a>.
	</p>
</div>

## Syntax

{% highlight csharp %}
PreCompile(
	string webApplicationName,
	string webApplicationPhysicalPath,
	string preCompileOutputpath
)
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
			<td>webApplicationName</td>
			<td>Name of the virtual application as it will be in IIS.</td>
		</tr>
		<tr>
			<td>webApplicationPhysicalPath</td>
			<td>Path to the application directory you want to compile. Support relative path.</td>
		</tr>
		<tr>
			<td>preCompileOutputpath</td>
			<td>Path to where the operation should output the pre-compiled result. Support relative path.</td>
		</tr>
	</tbody>
</table>

## Code Example
{% highlight csharp %}
public class MyLocalOperation : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Local.PreCompile("MyWebApp", @"C:\MyWebApp", @"C:\Compile\MyWebApp");
    }
}
{% endhighlight %}
