---
layout: doc-5-0
title: Load Balancer Integration
permalink: /docs/5-0/custom-loadbalancer/
---

To make a custom load balancer plugin to ConDep is very easy, you just ake a class that implements the interface ILoadBalance located in the ConDep.Dsl:

{% highlight csharp %}
public interface ILoadBalance
{
    Result BringOffline(string serverName, string farm, LoadBalancerSuspendMethod suspendMethod);
    Result BringOnline(string serverName, string farm);
    LoadBalancerMode Mode { get; set; }
    LoadBalanceState GetServerState(string serverName, string farm);
}
{% endhighlight %}

Take a look at [ConDep.Dsl.LoadBalancer.AlohaHaProxy](https://github.com/condep/condep-loadbalancer-haproxy). This is a good example on how you can make your own load balancer plugin.