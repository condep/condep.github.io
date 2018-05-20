---
layout: doc-5-0
title: SslCertificate().FromFile
permalink: /docs/5-0/deployment/ssl-certificate-from-file/
version_added: 3.1.0
---

Will deploy certificate from local file path given correct password for private key, and deploy to certificate store on remote server.

## Syntax

{% highlight csharp %}
FromFile(string path, string password)
{% endhighlight %}

{% highlight csharp %}
FromFile(string path, string password, Action<IOfferCertificateOptions> options)
{% endhighlight %}

<table>
	<thead>
		<tr>
			<th>Parameter</th>
			<th>Description</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>path</td>
			<td>Local path to certificate on disk</td>
		</tr>
		<tr>
			<td>password</td>
			<td>Password for private key</td>
		</tr>
		<tr>
			<td>options</td>
			<td>See <a href="../../options/IOfferCertificateOptions/">IOfferCertificateOptions</a> for details.</td>
		</tr>
	</tbody>
</table>

## Code Example

{% highlight csharp %}
public class MyRemoteOperation : Runbook
{
    public override void Execute(IOfferOperations dsl, ConDepSettings settings)
    {
        dsl.Remote(remote => remote.Deploy.SslCertificate().FromFile(
            path: @"C:\mycerts\mySslCert.pfx",
            password: "my$uper$ecretP@ssw0rd",
                options: opt => opt.AddPrivateKeyPermission(@"mydomain\myUser")
            ));
    }
}
{% endhighlight %}
