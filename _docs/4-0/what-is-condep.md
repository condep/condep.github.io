---
layout: doc-4-0
title: What is ConDep?
next_section: quick-start
permalink: /docs/4-0/what-is-condep/
---

ConDep is a open source infrastructure configuration and deployment DSL
(Domain Specific Language) specifically targeted to (but not limited to)
the Windows Server platform. If your familiar with tools like Chef, Puppet or Ansible, ConDep does very much the same, but with native support for Windows.

### Continuous Deployment/Delivery
Native to ConDep is Continuous Deployment (hence the name). We've made it
very easy to continuously deploy your software to your infrastructure -
whether that's a web server, application server, database server, or any other
type of server with a Windows Server OS.

### Infrastructure as Code
ConDep can do anything from bootstrapping virtual servers (typically in the cloud) to configuring and installing software on Windows Servers. ConDep started as a pure deployment tool, but over time evolved into a complete
infrastructure management tool (sometimes referred to as Infrastructure as Code). So what does "infrastructure management" mean? Since ConDep is built specifically to tackle the challenges on Windows, the most common tasks are really easy to do. Examples of Infrastructure Operations are IIS configuration, SSL certificate management, turning Windows features on/off, software installations (like MSI's) and rebooting servers.

### Environments, Tiers, Servers and Runbooks
<img src="../../../images/env_srvs_tiers.svg" class="img-align-right" style="width: 50%">
ConDep has the concept of [Environments](../environment/) and [Tiers](../environment/).
Environments contain Tiers, and Tiers contain servers (see drawing). Many organizations have multiple environments (like test, QA and production). Within your Environment configuration, you specify the Tiers it has (web-tier, app-tier, database-tier, etc), and which servers are located in these tiers.

After you've configured your Environments and Tiers, you use ConDep's Domain Specific Language to create your [Runbooks](../artifacts/). A Runbook could be deploying a web-application, or configuring a Web Server. It's up to you what you do inside a Runbook.

When you execute ConDep, you specify which Runbook to execute and to which Environment. For instance, you could execute your Runbook for your Web Application which is linked to the Web Tier in your Production Environment. Your Web Application will then be deployed to all servers in the Web Tier of your Production Environment.

### Intelligent
ConDep has some really nice intelligence built-in especially around deployment. ConDep knows, for instance, what a Load Balancer is, and how to operate it to enable **no-downtime deployments**.

### Push based
ConDep very similar to Ansible in that it is "push" based, meaning it will push infrastructure configuration and deployments to remote servers from a central place (like a CI server). This is something you need to be aware of when evaluating ConDep for your organization. Depending on your requirements (your network and firewalls), a push-based model might be exactly what you need, but some need/prefer a pull-based model instead. Tools like [Chef](https://www.chef.iopu) or [Puppet](http://puppetlabs.com) might help you if that's your scenario. For more information about ConDep's push-based model, check out [What does "push-based" mean?](/docs/4-0/push-based/).

### Idempotent
All [Operations](/docs/4-0/condep-dsl-operations/) in ConDep are **idempotent**, meaning you can run them as many times as you want and expect the same result. In practice this means if configure a IIS Web Site with certain settings, and somebody later manually alters those settings on the server, the next time ConDep is executed the settings will be reverted back.

This is also true for deployed files and directories. ConDep does not just copy them, it **synchronizes** them. So if you have deployed a folder, and someone later adds a file, ConDep will remove it on next execution.

That Operations are idempotent is a vital requirement for Continuous Deployment tools, and if you create your own Operations for ConDep, you should make sure they are idempotent too.

<div class="note warning">
	<h2>Scripts are NOT Idempotent</h2>
  <p>
	...unless you make them. Since ConDep can't control the Script's content, it's impossible to ensure idempotency. So if you create scripts that will be executed by ConDep, make sure they are Idempotent or make it clear they are not.
	</p>
</div>
