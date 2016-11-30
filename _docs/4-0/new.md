---
layout: doc-4-0
title: What's New in Version 4.0?
prev_section: quick-start
next_section: requirements
permalink: /docs/4-0/new/
---

In version 4.0 we made some major improvements to how the core execution engine of ConDep works. We've already seen this make ConDep more intuitive to use, but the drawback is some breaking changes (more details below).

In version 3.0 we explicitly invited the open source community to contribute more to ConDep, and that has paid off. We now have an active group of contributors ensuring the future for ConDep. We even have a dedicated user group in Bergen, Norway that meets regularly to contribute code, suggest improvements and fix bugs. Please contact us or send us pull requests if you want to be involved.

## Major changes

### Renamed Artifact to Runbook
This was a long time in the waiting. It was just too confusion and not very logical to call it an Artifact when in reality it was a Runbook :-) For those unfamiliar to ConDep, a Runbook is where you define the actions to be executed by ConDep.

### Changed execution model from _Delayed Execution_ to _Direct Execution_.
Earlier the sequence of operations you had in your Runbook (previously named Artifact) was added to a execution sequence and then executed at a later point in time. This had some implications on what you could do in your Runbook and sometimes caused confusion. One of the main problems was that all Local Operations was always executed first, no matter when in your Runbook they were defined. This is no longer an issue. The current Runbook with its direct execution model will be executed as is and you can even debug it step-by-step, which was not possible before.

### Load Balancing is now an Operation

## Breaking changes

* .NET Framework is upgraded from 4.0 to 4.5
* Artifact no longer exist and is replaced by Runbook
* Previous custom operations had

## A short version history

### Version 1.0
A very basic version built on top of Microsoft WebDeploy.

### Version 2.0-beta
This version was never officially released and was only available on NuGet as a
pre-release package. However, this was the stable version for years and the
one most organizations used for Continuous Deployment. This was also the version where all the invention happened and where ConDep early moved away from Microsoft WebDeploy over to PowerShell remoting.

### Version 3.0
In version 3.0 parts of the core DSL was rewritten and several parts (like ConDep's core Operations) was refactored out and into a separate assembly. The way custom Operations are created was also changed, so it was no difference between how ConDep's core Operations were created and how your own custom Operations were created. In GitHub we also split the ConDep repo into separate repos for all the different ConDep modules.

The most important part however was that 3.0 introduced a whole set of new Infrastructure operations which basically took ConDep from Deployment only to Infrastructure as Code, but bringing with it the strong foundation around deploying web applications.

### Version 4.0

## Minor and Patch changes after 4.0

None yet.
