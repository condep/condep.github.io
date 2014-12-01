---
layout: doc
title: What is ConDep?
next_section: new
permalink: /docs/3-0/what-is-condep/
---

<h1>What is <span style="color: #61c10b;">con</span>dep?</h1>

ConDep is a open source infrastructure configuration and deployment DSL (Domain Specific Language) specifically targeted to (but not limited to) the Windows Server platform. If your familiar with tools like Chef and Puppet, ConDep does very much the same, but with native support for Windows.

###Continuous Deployment/Delivery
Native to ConDep is Continuous Deployment (hence the name). We've made it very easy to continuously deploy your software to your infrastructure. That being a web server, application server, database server or any other server with a Windows Server OS.

###Infrastructure as Code
ConDep started as a pure deployment tool, but over time evolved into a complete infrastructure management tool (sometimes referred to as Infrastructure as Code). So what does "infrastructure management" mean? In short, it means it can do anything from bootstrapping virtual servers (typically in the cloud) to configuring and installing software on Windows Servers. Since ConDep is built specifically to tackle the challenges on Windows, the most common tasks are made really easy to do. Examples of Infrastructure Operations are IIS configuration, SSL certificate management, turning Windows features on/off, software installations (like MSI's) and rebooting servers.

<img src="../../../images/condep_arch3.png" class="img-align-right" style="shape-outside: url(../../../images/condep_arch3.png);shape-image-threshold: 0.1;">

### Environments, Tiers, Servers and Artifacts
ConDep has the concept of [Environments](../environment/) and [Tiers](../environment/). In short, Environments have Tiers, and Tiers have Servers (see drawing). Many organizations have multiple environments like test, qa and prod. Within your environment configuration you specify the tiers you have (web-tier, app-tier, database-tier etc) and which servers are located in these tiers. 

Then you create your [Artifacts](../artifacts/). This is where you use the ConDep Domain Specific Language to instrument what to do for a certain Artifact. An Artifact could be deploying an web-application or configure a Web Server. It's up to you what you do inside an artifact. Artifacts can be linked to Tiers.

When you execute ConDep you specify which Artifact to execute and to which Environment. E.g. you could execute your Artifact for your Web Application which is linked to the Web-Tier in your Prod environment. Your Web Application will then be deployed to all servers in the Web-Tier of your Prod environment.

###Intelligent
Where ConDep might differ from other infrastructure management tools is its built in intelligence around deployment of Web Applications. ConDep knows for instance what a Load Balancer is, and how to operate it to enable no downtime deployments. 

###Push based
ConDep is push based, meaning it will from a central place (like a CI server) push infrastructure configuration and deployments to remote servers. This is something you need to be aware of when evaluating ConDep for your organization. Depending on your requirements a push based model might be exactly what you need, but some need/prefer a pull based model instead. Tools like Chef or Puppet might help you if that's your scenario. For more information about ConDep's push based model, check out <a href="/docs/3-0/push-based/">What does push based mean?</a>.

###Idempotent
All Operations in ConDep are Idempotent, meaning you can run them as many times as you want and expect the same result. In practice this means if configure a IIS Web Site with certain settings, and somebody manually alter these settings on the server, the next time ConDep is executed the settings will be reverted back.

This is also true for deployed files and directories. ConDep does not just copy them, it synchronize them. So if you have deployed a folder and somebody adds a file manually, ConDep will remove it on next execution.

That Operations are Idempotent is vital and you will likely find the same for other similar tools, and if you create your own Operations for ConDep you should make sure they are Idempotent too.

<div class="note warning">
	<h2>Scripts are NOT Idempotent</h2>
  <p>
  	...unless you make them. Since ConDep can't control the Script's content, it's impossible to ensure idempotency. So if you create scripts that will be executed by ConDep, make sure they are Idempotent or make it clear they are not.
	</p>
</div>
