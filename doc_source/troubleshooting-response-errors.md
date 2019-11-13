# Troubleshooting Error Responses from Your Origin<a name="troubleshooting-response-errors"></a>

If CloudFront requests an object from your origin, and the origin returns an HTTP 4xx or 5xx status code, there's a problem with communication between CloudFront and your origin\. The following topics describe common causes for some of these HTTP status codes, and some possible solutions\.

**Topics**
+ [HTTP 400 Status Code \(Bad Request\)](http-400-bad-request.md)
+ [HTTP 500 Status Code \(Lambda Execution Error\)](http-500-lambda-execution-error.md)
+ [HTTP 502 Status Code \(Bad Gateway\)](http-502-bad-gateway.md)
+ [HTTP 502 Status Code \(Lambda Validation Error\)](http-502-lambda-validation-error.md)
+ [HTTP 503 Status Code \(Lambda Limit Exceeded\)](http-503-lambda-limit-execeeded-error.md)
+ [HTTP 503 Status Code \(Service Unavailable\)](http-503-service-unavailable.md)
+ [HTTP 504 Status Code \(Gateway Timeout\)](http-504-gateway-timeout.md)