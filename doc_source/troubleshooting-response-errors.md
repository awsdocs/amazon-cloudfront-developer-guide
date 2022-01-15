# Troubleshooting error responses from your origin<a name="troubleshooting-response-errors"></a>

If CloudFront requests an object from your origin, and the origin returns an HTTP 4xx or 5xx status code, there's a problem with communication between CloudFront and your origin\. The following topics describe common causes for some of these HTTP status codes, and some possible solutions\.

**Topics**
+ [HTTP 400 status code \(Bad Request\)](http-400-bad-request.md)
+ [HTTP 500 status code \(Lambda execution error\)](http-500-lambda-execution-error.md)
+ [HTTP 502 status code \(Bad Gateway\)](http-502-bad-gateway.md)
+ [HTTP 502 status code \(Lambda validation error\)](http-502-lambda-validation-error.md)
+ [HTTP 503 status code \(Lambda limit exceeded\)](http-503-lambda-limit-execeeded-error.md)
+ [HTTP 503 status code \(Service Unavailable\)](http-503-service-unavailable.md)
+ [HTTP 504 status code \(Gateway Timeout\)](http-504-gateway-timeout.md)