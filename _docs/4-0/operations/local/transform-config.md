---
layout: doc
title: TransformConfig
prev_section: 3-0/operations/local/pre-compile
next_section: 3-0/operations/deployment/directory
permalink: /3-0/local/transform-config/
version_added: 3.1.0
---

This operation transforms .NET configuration files (web and app config), in exactly the same way as msbuild or Visual Studio does.

<div class="note info">
	<h2>Transform configurations as part of Deployment, NOT Build!</h2>
  <p>
		We believe that doing transforms as part of the build is an anti-pattern and forces you to do a new build for every environment. Do config transformations as part of the deployment process instead.
	</p>
</div>

Here’s the process we recommend when deploying to multiple environments (e.g. test, pre-prod and prod):

1. Build
2. Transform config file for dev
3. Deploy dev
4. Transform config file for test
5. Deploy test
6. Transform config file for prod
7. Deploy prod

Not only does this avoid spending time building the same code for every environment, but will let you deploy the exact same binaries to all environments.

Before ConDep executes the transformation the first time, it makes a copy of the original config file with the extension .orig.condep. So a `web.config` file will be backed up as `web.config.orig.condep`. For all transforms succeeding this (e.g. test environment), ConDep will use the `web.confg.orig.condep` file as the config name, to guarantee multiple successful transformations in the same file set.

## Syntax

{% highlight csharp %}
TransformConfigFile(
  string configDirPath,
  string configName,
  string transformName)
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
			<td>configDirPath</td>
			<td>The directory where the config file and transform file(s) are located. Support relative path.</td>
		</tr>
		<tr>
			<td>configName</td>
			<td>Name of the config file to transform (e.g. <code>web.config</code>).</td>
		</tr>
		<tr>
			<td>transformName</td>
			<td>Name of the config transform file (e.g. <code>web.dev.config</code>).</td>
		</tr>
	</tbody>
</table>

## Code Example

{% highlight csharp %}
public class MyLocalArtifact : Artifact.Local
{
	public override void Configure(IOfferLocalOperations onLocalMachine, ConDepConfig config)
	{
	    onLocalMachine
	        .TransformConfigFile(@"C:\MyApp", "web.config", "web.test.config");
	}
}
{% endhighlight %}
