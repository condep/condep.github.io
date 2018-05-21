---
layout: doc-5-0
title: Load Balancer
prev_section: condep-exe
permalink: /docs/5-0/load-balancer/
---

ConDep supports deploying applications that are load balanced. Here is an example on how to deploy a directory to servers that are load balanced:

{% highlight csharp %}
public class MyApp : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {   
        dsl.LoadBalance(LoadBalancerMode.Sticky, "MyWebFarm", remote => 
        	remote.Deploy.Directory(
                sourceDir: @"C:\Apps\MyApp",
                destDir: @"E:\Apps\MyApp"));   
    }
}
{% endhighlight %}

You configure load balancer plugin in the [env_name].env.json config file. In the configuration file you can set these variables when setting up the loadbalancer:

- Name: The loadbalansers server name or IP.  
- Provider: The name of the dll that contains the load balancer plugin.  
- UserName: The username to a load balancer user that has access to take servers in and out of a server farm.  
- Password: The users password.  
- SuspendMode: Tell how to remove server from farm. The alternatives is graceful, suspendclearconnections or suspend. This setting is optional.  
- Mode: Alternatives are sticky and roundrobin.  
- TimeoutInSeconds: Should be as long as the application request timeout, waiting for connections to drain.  
- CustomConfig: A section that can be used to whatever you like.  

At the moment ConDep just supports HAProxy, but you can easily crete your own loadbalancer plugin. To see how to do that, take a look at [Load Balancer Integration](/docs/5-0/custom-loadbalancer) found in the Extending section.

### Using Aloha HAProxy

Install nuget package [ConDep.Dsl.LoadBalancer.AlohaHaProxy](https://www.nuget.org/packages/ConDep.Dsl.LoadBalancer.AlohaHaProxy/) in your project. [Here](https://github.com/condep/condep-loadbalancer-haproxy) is where you can find the load balancer plugin on Github.

Here is an example of what the [env_name].env.json config file might look like when using Aloha HAProxy plugin:

{% highlight json %}
{
   "LoadBalancer": {
        "Name": "http://10.38.38.100:4444",
        "Provider": "ConDep.Dsl.LoadBalancer.AlohaHaProxy.dll",
        "UserName": "username",
        "Password": "password",
        "Mode": "Sticky",
        "TimeoutInSeconds": "60",
        "CustomConfig": {
            "SnmpEndpoint": "10.38.38.100",
            "SnmpPort": "161",
            "SnmpCommunity": "public"
	        "WaitTimeInSecondsAfterSettingServerStateToOffline": 60,
	        "WaitTimeInSecondsAfterSettingServerStateToOnline": 2
        }
    }
}
{% endhighlight %}