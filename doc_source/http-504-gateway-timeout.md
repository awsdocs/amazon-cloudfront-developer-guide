# HTTP 504 Status Code \(Gateway Timeout\)<a name="http-504-gateway-timeout"></a>

An HTTP 504 status code \(Gateway Timeout\) indicates that when CloudFront forwarded a request to the origin \(because the requested object wasn’t in the edge cache\), one of the following happened:
+ The origin returned an HTTP 504 status code to CloudFront\.
+ The origin didn’t respond before the request expired\.

CloudFront will return an HTTP 504 status code if traffic is blocked to the origin by a firewall or security group, or if the origin isn’t accessible on the internet\. Check for those issues first\. Then, if access isn’t the problem, explore application delays and server timeouts to help you identify and fix the issues\.

**Topics**
+ [Configure the Firewall on Your Origin Server to Allow CloudFront Traffic](#http-504-gateway-timeout-configure-firewall)
+ [Configure the Security Groups on Your Origin Server to Allow CloudFront Traffic](#http-504-gateway-timeout-configure-security-groups)
+ [Make Your Custom Origin Server Accessible on the Internet](#http-504-gateway-timeout-make-origin-accessible)
+ [Find and Fix Delayed Responses from Applications on Your Origin Server](#http-504-gateway-timeout-slow-application)

## Configure the Firewall on Your Origin Server to Allow CloudFront Traffic<a name="http-504-gateway-timeout-configure-firewall"></a>

If the firewall on your origin server blocks CloudFront traffic, CloudFront returns an HTTP 504 status code, so it's good to make sure that isn’t the issue before checking for other problems\.

The method that you use to determine if this is an issue with your firewall depends on what system your origin server uses:
+ If you use an IPTable firewall on a Linux server, you can search for tools and information to help you work with IPTables\.
+ If you use Windows Firewall on a Windows server, see [ Add or Edit Firewall Rule](https://technet.microsoft.com/en-us/library/cc753558(v=ws.11).aspx) on Microsoft Technet\.

When you evaluate the firewall configuration on your origin server, look for any firewalls or security rules that block traffic from CloudFront edge locations, based on the [ published IP address range](https://ip-ranges.amazonaws.com/ip-ranges.json)\.

If the CloudFront IP address range is whitelisted on your origin server, make sure to update your server’s security rules to incorporate changes\. You can subscribe to an Amazon Simple Notification Service \(SNS\) topic and receive notifications when the IP address range file is updated\. After you receive the notification, you can use code to retrieve the file, parse it, and make adjustments for your local environment\." For more information, see [ Subscribe to AWS Public IP Address Changes via Amazon SNS](https://aws.amazon.com/blogs/aws/subscribe-to-aws-public-ip-address-changes-via-amazon-sns/)\. 

## Configure the Security Groups on Your Origin Server to Allow CloudFront Traffic<a name="http-504-gateway-timeout-configure-security-groups"></a>

If your origin uses Elastic Load Balancing, review the [ ELB security groups](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-security-groups.html) and make sure that the security groups allow inbound traffic from CloudFront\.

You can also use AWS Lambda to [ automatically update your security groups](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-security-groups.html) to allow inbound traffic from CloudFront\.

## Make Your Custom Origin Server Accessible on the Internet<a name="http-504-gateway-timeout-make-origin-accessible"></a>

If CloudFront can’t access your custom origin server because it isn’t publicly available on the internet, CloudFront returns an HTTP 504 error\.

CloudFront edge locations connect to origin servers through the internet\. If your custom origin is on a private network, CloudFront can’t reach it\. Because of this, you can’t use private servers, including [ internal Classic Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-internal-load-balancers.html), as origin servers with CloudFront\.

To check that internet traffic can connect to your origin server, run the following commands \(where OriginDomainName is the domain name for your server\):

For HTTPS traffic:
+ nc \-zv OriginDomainName 443
+ telnet OriginDomainName 443

For HTTP traffic:
+ nc \-zv OriginDomainName 80
+ telnet OriginDomainName 80

## Find and Fix Delayed Responses from Applications on Your Origin Server<a name="http-504-gateway-timeout-slow-application"></a>

Server timeouts are often the result of either an application taking a very long time to respond, or a timeout value that is set too low\.

A quick fix to help avoid HTTP 504 errors is to simply set a higher CloudFront timeout value for your distribution\. But we recommend that you first make sure that you address any performance and latency issues with the application and origin server\. Then you can set a reasonable timeout value that helps prevent HTTP 504 errors and provides good responsiveness to users\.

Here’s an overview of the steps you can take to find performance issues and correct them:

1. Measure the typical and high\-load latency \(responsiveness\) of your web application\.

1. Add additional resources, such as CPU or memory, if needed\. Take other steps to address issues, such as tuning database queries to accommodate high\-load scenarios\.

1. If needed, adjust the timeout value for your CloudFront web distribution\.

Following are details about each step\.

### Measure typical and high\-load latency<a name="http-504-gateway-timeout-slow-application-measure-latency"></a>

To determine if one or more backend web application servers are experiencing high latency, run the following Linux curl command on each server:

`curl -w "Connect time: %{time_connect} Time to first byte: %{time_starttransfer} Total time: %{time_total} \n" -o /dev/null https://www.example.com/yourobject`

**Note**  
If you run Windows on your servers, you can search for and download curl for Windows to run a similar command\.

As you measure and evaluate the latency of an application that runs on your server, keep in mind the following:
+ Latency values are relative to each application\. However, a Time to First Byte in milliseconds rather than seconds or more, is reasonable\.
+ If you measure the application latency under normal load and it’s fine, be aware that viewers might still experience timeouts under high load\. When there is high demand, servers can have delayed responses or not respond at all\. To help prevent high\-load latency issues, check your server’s resources such as CPU, memory, and disk reads and writes to make sure that your servers have the capacity to scale for high load\.

  You can run the following Linux command to check the memory that is used by Apache processes:

  `watch -n 1 "echo -n 'Apache Processes: ' && ps -C apache2 --no-headers | wc -l && free -m"`
+ High CPU utilization on the server can significantly reduce an application’s performance\. If you use an Amazon EC2 instance for your backend server, review the CloudWatch metrics for the server to check the CPU utilization\. For more information, see the [ Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)\. Or if you’re using your own server, refer to the server Help documentation for instructions on how to check CPU utilization\.
+ Check for other potential issues under high loads, such as database queries that run slowly when there’s a high volume of requests\.

### Add resources, and tune servers and databases<a name="http-504-gateway-timeout-slow-application-add-resources"></a>

After you evaluate the responsiveness of your applications and servers, make sure that you have sufficient resources in place for typical traffic and high load situations:
+ If you have your own server, make sure it has enough CPU, memory, and disk space to handle viewer requests, based on your evaluation\.
+ If you use an Amazon EC2 instance as your backend server, make sure that the instance type has the appropriate resources to fulfill incoming requests\. For more information, see [ Instance Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) in the Amazon EC2 User Guide\. 

In addition, consider the following tuning steps to help avoid timeouts:
+ If the Time to First Byte value that is returned by the curl command seems high, take steps to improve the performance of your application\. Improving application responsiveness will in turn help reduce timeout errors\.
+ Tune database queries to make sure that they can handle high request volumes without slow performance\.
+ Set up [ keep\-alive \(persistent\)](https://www.w3.org/Protocols/HTTP/1.1/draft-ietf-http-v11-spec-01) connections on your backend server\. This option helps to avoid latencies that occur when connections must be re\-established for subsequent requests or users\.
+ If you use ELB as your origin, learn how you can reduce latency by reviewing the suggestions in the following Knowledge Center article: [ How to troubleshoot ELB high latency](https://aws.amazon.com/premiumsupport/knowledge-center/elb-latency-troubleshooting/)\.

### If needed, adjust the CloudFront timeout value<a name="http-504-gateway-timeout-slow-application-adjust-timeout"></a>

If you have evaluated and addressed slow application performance, origin server capacity, and other issues, but viewers are still experiencing HTTP 504 errors, then you should consider changing the time that is specified in your web distribution for origin response timeout\. To learn more, see [ Origin Response Timeout](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesOriginResponseTimeout)\.