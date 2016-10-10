---
layout: doc
title: What's New in Version 4.0?
prev_section: quick-start
next_section: requirements
permalink: /docs/4-0/new/
---

We believe with version 3.0 that ConDep has evolved into the best Continuous Deployment
platform on Windows, and we want the World to know!

<div class="note info">
  <p>
		In this version we really want to involve the open source community a lot more.
		Make no mistake about it, we would really like your input, feedback and contributions!
	</p>
</div>

## New features

### Changed execution model from *Delayed Execution* to *Direct Execution*.
Previously the sequence of operations you had in your Artifact (now called Runbook) was added to
a execution sequence and then executed at a later point. This had some implications on what you could do in
your Runbook and sometimes caused some confusion.

The new Runbook will now be executed directly as is. ...


## Breaking changes

* .NET Framework is upgraded from 4.0 to 4.5
* Artifact no longer exist and is replaced by Runbook
* Previous custom operations had

## A short version history

### Version 1.0
A very basic version built on top of Microsoft WebDeploy. This is the public release
that's been available on NuGet for several years.

### Version 2.0-beta
This version was never officially released and was only available on NuGet as a
pre-release package. However, this has been the stable version for years and is the
one that we have used for Continuous Deployment. This is also the version where all
the invention happened and where ConDep early moved away from Microsoft WebDeploy over
to PowerShell remoting.

### Version 3.0
In version 3.0 parts of the core DSL was rewritten and several parts (like ConDep's
core Operations) was refactored out and into a separate assembly. The way we create
custom Operations was also changed, so there is now no difference between how ConDep's
core Operations are created verses how your own custom Operations are created. Also in
GitHub we split the ConDep repo into separate repos for all ConDep projects.

The most important part however was that 3.0 introduced a whole set of new Infrastructure
operations which basically took ConDep from Deployment only to Infrastructure as Code,
but bringing with it the strong knowledge it had around deploying web applications.

<div class="note info">
	<h2>Future Version Changes</h2>
  <p>
		From version 3.0 and onwards we will track feature changes and have the documentation
		updated for every new version not in Beta.
	</p>
</div>

### Version 4.0

## Minor and Patch changes after 3.0

### ConDep.Dsl

**3.1.3**

* Fixed a bug releated to Tiers in env.json. If Tiers was used in the Environment file,
it prevented execution of remote operations ([bug #4](https://github.com/condep/condep-dsl/issues/4)).
Thanks to [@rubenmamo](https://github.com/rubenmamo) for reporting bug and validating fix.

### ConDep.Dsl.Operations

**3.1.1**

* Fixed a bug in MSI and Custom installers where validation of existing installation was
not correct (re-installed even if already installed). Thanks to
[@kjelliverb](https://github.com/condep/condep-dsl-operations/pulls/kjelliverb) for
sorting this out.

**3.1.2**

* In IISAppPoolOptions depcrecated IdentityUsername() and IdentityPassword() in favor of
a new Identity property. This property gives simple methods for setting common identities
like NetworkService, LocalService, LocalSystem etc. in addition to SpecificUser(). Thanks
to [@djcrabhat](https://github.com/djcrabhat) for committing the necessary PowerShell
script changes.
