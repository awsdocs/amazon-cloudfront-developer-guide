# Add cross\-origin resource sharing \(CORS\) header to the request<a name="example-function-add-cors-header-request"></a>

The following example function adds an `Origin` HTTP header to the request if the request doesn’t already contain this header\. This header is part of [cross\-origin resource sharing \(CORS\)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)\. This example sets the header’s value to the value in the request’s `Host` header\. For more information, see [Origin](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Origin) on the MDN Web Docs website\.

This is a viewer request function\.

[See this example on GitHub](https://github.com/aws-samples/amazon-cloudfront-functions/tree/main/add-origin-header)\.

```
function handler(event) {
    var request = event.request;
    var headers = request.headers;
    var host = request.headers.host.value;

   // If origin header is missing, set it equal to the host header.
   if (!headers.origin)
       headers.origin = {value:`https://${host}`};

   return request;
}
```