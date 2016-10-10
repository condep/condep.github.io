---
layout: doc-4-0
title: Push based
prev_section: architecture
next_section: environment
permalink: /docs/4-0/push-based/
---

ConDep is what we refer to as a **Push Based** system, meaning it will push deployment
and infrastructure configurations instead of pulling them. In other words, ConDep is
about **Remote Execution**. Systems like [Chef](http://www.chef.io) and [Puppet](http://puppetlabs.com/)
have agents running on all managed servers that will **Pull** resources down from a
central server and execute locally. This is what we refer to as **Pull Based** systems.
In that way ConDep is closer to [Ansible](http://www.ansible.com/).

There are several differences between the two models, but the major difference you
need to be aware of is that **Push Based** systems needs network access to its managed
servers (servers to do remote execution on). In **Pull Based** systems it is the managed
servers that must have access to the source server, to pull resources to execute locally.
This is why for example Chef can provide a Hosted Chef in the cloud.

Usually **Pushed Based** systems are simpler to setup and configure, have very few
requirements, intuitive and lets you quickly get started. This does however not help if
your network topology prevent you from using **Push Based** systems.
