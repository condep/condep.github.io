---
layout: doc
title: SslCertificate().FromStore
permalink: /3-0/deployment/ssl-certificate-from-store/
version_added: 3.1.0
---

Will search for certificate in local cert store using the <a href="http://msdn.microsoft.com/en-us/library/vstudio/system.security.cryptography.x509certificates.x509findtype(v=vs.100).aspx">X509FindType</a> and a <code>findValue</code>, and deploy certificate to certificate store on remote server.

## Syntax

{% highlight csharp %}
FromStore(X509FindType findType, string findValue)
{% endhighlight %}

{% highlight csharp %}
FromStore(X509FindType findType, string findValue, Action<IOfferCertificateOptions> options)
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
			<td>findType</td>
			<td>The <a href="http://msdn.microsoft.com/en-us/library/vstudio/system.security.cryptography.x509certificates.x509findtype(v=vs.100).aspx">X509FindType</a> to use for finding certificate in store.</td>
		</tr>
		<tr>
			<td>findValue</td>
			<td>The value to search for using the <a href="http://msdn.microsoft.com/en-us/library/vstudio/system.security.cryptography.x509certificates.x509findtype(v=vs.100).aspx">X509FindType</a>.
			</td>
		</tr>
		<tr>
			<td>options</td>
			<td>See <a href="../../options/IOfferCertificateOptions/">IOfferCertificateOptions</a> for details.</td>
		</tr>
	</tbody>
</table>

## Code Example

{% highlight csharp %}
public class MyRemoteArtifact : Artifact.Remote
{
  public override void Configure(IOfferRemoteOperations server, ConDepSettings settings)
  {
    server.Deploy
      .SslCertificate().FromStore(
        findType: X509FindType.FindByThumbprint,
        findValue: "82 b5 52 2e 70 71 09 e0 ab 44 a0 4b b6 1a 4b 72 bf f7 12 de",
        options: opt => opt.AddPrivateKeyPermission(@"mydomain\myUser")
      );
  }
}
{% endhighlight %}
