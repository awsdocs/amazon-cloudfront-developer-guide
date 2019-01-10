# Troubleshooting Error Responses from Your Origin<a name="troubleshooting-response-errors"></a>

If CloudFront requests an object from your origin, and the origin returns an HTTP 4xx or 5xx status code, there's a problem with communication between CloudFront and your origin\. The following sections describe common causes for selected HTTP status codes and provides some possible solutions\.

**Important**  
If you're a customer trying to access a website or application, and you've gotten an error from CloudFront, there's probably just unusually high traffic to the site you're trying to access\. Please wait a little while, and then try accessing the site \(or running the application\) again\. If you still get an error, please contact the website or application distributor directly for support\.   
**Why is this error coming from CloudFront?** CloudFront helps websites speed up delivery of content, like images or web pages, to customers by storing copies in servers located around the world\. But when there's a lot of internet traffic to a website and the site can't keep up, an error is returned when anyone tries to access the site\. When CloudFront can't access content that you've requested from a website, it passes on the error from the site or application that you're trying to use\.

**Topics**
+ [HTTP 502 Status Code \(Bad Gateway\)](http-502-bad-gateway.md)
+ [HTTP 503 Status Code \(Service Unavailable\)](http-503-service-unavailable.md)
+ [HTTP 500 Status Code \(Lambda Execution Error\)](http-503-lambda-execution-error.md)
+ [HTTP 502 Status Code \(Lambda Validation Error\)](http-503-service-unavailable-lambda-function-invalid.md)
+ [HTTP 503 Status Code \(Lambda Limit Exceeded\)](http-503-lambda-limit-execeeded-error.md)
+ [HTTP 504 Status Code \(Gateway Timeout\)](http-504-gateway-timeout.md)