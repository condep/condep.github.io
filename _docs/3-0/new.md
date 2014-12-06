---
layout: doc
title: What's new in version 3.0?
prev_section: what-is-condep
next_section: requirements
permalink: /docs/3-0/new/
---

What's new in version 3.0?
==========================

We believe with version 3.0 that ConDep has evolved into the best Continuous Deployment platform on Windows, and we want the World to know!  

<div class="note info">
  <p>
		In this version we really want to involve the open source community a lot more. Make no mistake about it, we would really like your input, feedback and contributions!
	</p>
</div>

## A short version history

### Version 1.0
A very basic version built on top of Microsoft WebDeploy. This is the public release that's been available on NuGet for several years.

### Version 2.0-beta
This version was never officially released and was only available on NuGet as a pre-release package. However, this has been the stable version for years and is the one that we have used for Continuous Deployment. This is also the version where all the invention happened and where ConDep early moved away from Microsoft WebDeploy over to PowerShell remoting.

### Version 3.0
In version 3.0 parts of the core DSL was rewritten and several parts (like ConDep's core Operations) was refactored out and into a separate assembly. The way we create custom Operations was also changed, so there is now no difference between how ConDep's core Operations are created verses how your own custom Operations are created. Also in GitHub we split the ConDep repo into separate repos for all ConDep projects.

The most important part however was that 3.0 introduced a whole set of new Infrastructure operations which basically took ConDep from Deployment only to Infrastructure as Code, but bringing with it the strong knowledge it had around deploying web applications.

<div class="note info">
	<h2>Future Version Changes</h2>
  <p>
		From version 3.0 and onwards we will track feature changes and have the documentation updated for every new version not in Beta.	
	</p>
</div>

## Minor and Patch changes after 3.0

### ConDep.Dsl

#### 3.0.3

* Fixed a bug when Tiers was used in the Environment file preventing execution of remote operations ([bug #4](https://github.com/condep/condep-dsl/issues/4)).
