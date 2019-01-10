# Troubleshooting<a name="Troubleshooting"></a>

Troubleshoot common problems you might encounter when setting up Amazon CloudFront to distribute your content or when using Lambda@Edge, and find possible solutions\.

**Important**  
If you're a customer trying to access a website or application, and you've gotten an error from CloudFront, there's probably just unusually high traffic to the site you're trying to access\. Please wait a little while, and then try accessing the site \(or running the application\) again\. If you still get an error, please contact the website or application distributor directly for support\.   
**Why is this error coming from CloudFront?** CloudFront helps websites speed up delivery of content, like images or web pages, to customers by storing copies in servers located around the world\. But when there's a lot of internet traffic to a website and the site can't keep up, an error is returned when anyone tries to access the site\. When CloudFront can't access content that you've requested from a website, it passes on the error from the site or application that you're trying to use\. 

**Topics**
+ [Troubleshooting Distribution Issues](troubleshooting-distributions.md)
+ [Troubleshooting Error Responses from Your Origin](troubleshooting-response-errors.md)
+ [Load Testing CloudFront](load-testing.md)