# HTTP 502 status code \(DNS error\)<a name="http-502-dns-error"></a>

An HTTP 502 error with the `NonS3OriginDnsError` error code indicates that there's a DNS configuration problem that prevents CloudFront from connecting to the origin\. If you get this error from CloudFront, make sure that the origin's DNS configuration is correct and working\.

When CloudFront receives a request for an object that's expired or is not in its cache, it makes a request to the origin to get the object\. To make a successful request to the origin, CloudFront performs a DNS resolution on the origin domain name\. If the DNS service for your domain is experiencing issues, CloudFront can't resolve the domain name to get the IP address, which results in an HTTP 502 error \(`NonS3OriginDnsError`\)\. To fix this problem, contact your DNS provider, or, if you are using Amazon Route 53, see [Why can't I access my website that uses Route 53 DNS services?](https://aws.amazon.com/premiumsupport/knowledge-center/route-53-dns-website-unreachable/)

To further troubleshoot this issue, ensure that the [authoritative name servers](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/route-53-concepts.html#route-53-concepts-authoritative-name-server) of your origin's root domain or zone apex \(such as `example.com`\) are functioning correctly\. You can use the following commands to find the name servers for your apex origin, with a tool such as [dig](https://en.wikipedia.org/wiki/Dig_(command)) or [nslookup](https://en.wikipedia.org/wiki/Nslookup):

```
dig OriginAPEXDomainName NS +short
```

```
nslookup -query=NS OriginAPEXDomainName
```

When you have the names of your name servers, use the following commands to query the domain name of your origin against them to make sure that each responds with an answer:

```
dig OriginDomainName @NameServer
```

```
nslookup OriginDomainName NameServer
```

**Important**  
Make sure that you perform this DNS troubleshooting using a computer that's connected to the public internet\. CloudFront resolves the origin domain name using public DNS on the internet, so it's important to troubleshoot in a similar context\.

If your origin is a subdomain whose DNS authority is delegated to a different name server than the root domain, make sure that the name server \(`NS`\) and start of authority \(`SOA`\) records are configured correctly for the subdomain\. You can check for these records using commands similar to the preceding examples\.

For more information about DNS, see [Domain Name System \(DNS\) concepts](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/route-53-concepts.html#route-53-concepts-domain-name-system-dns) in the Amazon Route 53 documentation\.