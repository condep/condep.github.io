---
layout: doc-4-0
title: DevOps Terminology
prev_section: requirements
next_section: best-practices
permalink: /docs/4-0/terms/
---

[<img src="http://ecx.images-amazon.com/images/I/71sYKaNItcL.jpg" class="img-align-right" style="width: 120px;">](http://www.amazon.com/dp/0321601912?tag=contindelive-20)
Below we have described some common terms that comes up in deployment and release
literature, and our meaning of it. Talking about literature, there is one book that
stands out in relation to delivering software, and ConDep highly embrace and
recommend reading it. It's written by Jez Humble ([@jezhumble](https://twitter.com/jezhumble))
and David Farley ([@davefarley77](https://twitter.com/davefarley77)), and
titled [Continuous Delivery: Reliable Software Releases through Build, Test and Deployment Automation](http://www.amazon.com/dp/0321601912?tag=contindelive-20).

## Continuous Deployment vs. Continuous Delivery

* Continuous Deployment is to deliver every successful build to production.
* Continuous Delivery is that every build can potentially be delivered to production,
  typically by the click of a button

## Deploy vs. Release

* You can deploy code to production without releasing
* Release is to make functionality available to end users

## Push vs. Pull

* Push is when deployment and infrastructure changes are pushed from a central place
  to the servers
* Pull is when each server queries a central place for deployment and infrastructure
  changes, and apply the changes locally

## Deployment Pipeline

From the [Continuous Deployment](http://www.amazon.com/dp/0321601912?tag=contindelive-20) book:

> At an abstract level, a deployment pipeline is an automated manifestation of your
> process for getting software from version control into the hands of your users.
