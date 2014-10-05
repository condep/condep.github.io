---
layout: page
title: What is ConDep?
next_section: new
permalink: /docs/what-is-condep/
---

What is ConDep?
===============

##Short version
ConDep enables you to push software and infrastructure configurations to Windows Servers. This can be deployment of your applications and/or complete setup for your Servers (infrastructure). Examples are software needed, IIS configuration, and other prerequisites for your application to work (this also includes other Servers). 

![ConDep Architecture Image](../../images/condep_arch.png)The idea is that ConDep controls the Server using an already bootstrapped Server, or bootstrap the server for you (typically in the cloud). Any changes needed on the Servers are added to ConDep, and will be effectuated the next time you run it.

You do this by creating Artifacts using ConDep's internal DSL in Microsoft .NET (most commonly C#) and execute these Artifacts with condep.exe, ConDep's command line tool. 

By creating these Artifacts for e.g. a Web Server, you can point ConDep to any Windows Server and it will configure it as the Web Server you have defined. Often though, you have more than just WebServers. This is why ConDep has the concepts of Environments and Tiers. In short, Environments have Tiers, and Tiers have Servers (see drawing). To read more about these concepts, see the documentation under the _Usage_ section in the documentation menu.

##Long(er) version
ConDep is a open source infrastructure configuration and deployment DSL (Domain Specific Language) specifically targeted to (but not limited to) the Windows Server platform. If your familiar with tools like Chef and Puppet, ConDep does very much the same, but with native support for Windows.

<div class="note warning">
  <p>
		ConDep is push based, meaning it will from a central place (like a CI server) push infrastructure configuration and deployments to remote servers. This is something you need to be aware of when evaluating ConDep for your organization. Depending on your requirements a push based model might be exactly what you need, but some need/prefer a pull based model instead. Tools like Chef or Puppet might help you if that's your scenario. For more information about ConDep's push based model, check out <a href="/docs/push-based/">What does push based mean?</a>.
	</p>
</div>

###Continuous Deployment/Delivery
Native to ConDep is Continuous Deployment (hence the name). That means we've made it very easy to continuously deploy your software to your infrastructure. That being a web server, application server or a database server.

###Infrastructure as Code
ConDep started as a pure deployment tool, but over time evolved into a complete infrastructure management tool (sometimes referred to as Infrastructure as Code). So what does "infrastructure management" mean? In short, it means it can do anything from bootstrapping virtual servers (typically in the cloud) to configuring and installing software on Windows Servers. Since ConDep is built specifically to tackle the challenges on Windows, the most common tasks are made really easy to do. Examples of Infrastructure Operations are IIS configuration, SSL certificate management, turning Windows features on/off, software installations (like MSI's) and rebooting servers.

###Intelligent
Where ConDep might differ from other infrastructure management tools is its built in intelligence around deployment of Web Applications. ConDep knows for instance what a Load Balancer is, and how to operate it to enable no downtime deployments. During configuration of ConDep you define your environments. Many organizations have multiple environments like test, qa and prod. In ConDep you create configuration files for each of your environments, telling ConDep about what tiers you have (web-tier, app-tier, database-tier etc) and which servers are located in these tiers. You then tell ConDep to execute an Artifact on a Tier. E.g. you could execute your Artifact for your Web Application on the Web-Tier.

###Idempotent
