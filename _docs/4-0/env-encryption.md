---
layout: doc-4-0
title: Environment Encryption
prev_section: requirements
next_section: quick-start
permalink: /docs/4-0/env-encryption/
---

Environment configuration files typically contains user names and passwords and other
sensitive data. To prevent this data to be added to source control or visible to anyone
that comes by the file, ConDep offers a simple command line switch for encryption/decryption.

Before encryption:

{% highlight json %}
{
  "Servers" :
  [
    {
      "Name" : "ec2-54-216-139-21.eu-west-1.compute.amazonaws.com"
    }
  ],
  "DeploymentUser":
  {
    "UserName": "Administrator",
    "Password": "ReallySecureP@$$w0rd :-)"
  }
}
{% endhighlight %}

After encryption:

{% highlight json %}
{
  "Servers": [
    {
      "Name": "ec2-54-216-139-21.eu-west-1.compute.amazonaws.com"
    }
  ],
  "DeploymentUser": {
    "UserName": "Administrator",
    "Password": {
      "IV": "dtDhog4UEaTNqhgekeXNHg==",
      "Password": "I9n4/dwU8WFGa/mUOcgSO6/DhfZsIaRdyl85w/a/N10="
    }
  }
}
{% endhighlight %}

The example above uses structured data known to ConDep (DeploymentUser), but what about
unstructured data that is sensitive? For these cases you can encapsulate your sensitive
key/value inside `encrypt`, and it will be encrypted together with the other data.
Let's say you have an environment file with some json data like this:

{% highlight json %}
{
  "SomeKey": "MySensitiveValue",
  "SomeOtherKey": "NotSensitiveValue"
}
{% endhighlight %}

To encrypt `SomeKey`'s `MySensitiveValue`, just encapsulate it with `encrypt` like this:

{% highlight json %}
{
  "SomeKey":
  {
    "encrypt" : "MySensitiveValue"
  },
  "SomeOtherKey": "NotSensitiveValue"
}
{% endhighlight %}

When encrypted it would look like this:

{% highlight json %}
{
  "SomeKey": {
    "IV": "dtDhog4UEaTNqhgekeXNHg==",
    "SomeKey": "I9n4/dwU8WFGa/mUOcgSO6/DhfZsIaRdyl85w/a/N10="
  },
  "SomeOtherKey": "NotSensitiveValue"
}
{% endhighlight %}
