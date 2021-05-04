# Add a `Cache-Control` header to the response<a name="example-function-add-cache-control-header"></a>

The following example function adds a `Cache-Control` HTTP header to the response\. The header uses the `max-age` directive to tell web browsers to cache the response for a maximum of two years \(63,072,000 seconds\)\. For more information, see [Cache\-Control](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) on the MDN Web Docs website\.

This is a viewer response function\.

[See this example on GitHub](https://github.com/aws-samples/amazon-cloudfront-functions/tree/main/add-cache-control-header)\.

```
function handler(event) {
    var response = event.response;
    var headers = response.headers;

    // Set the cache-control header
    headers['cache-control'] = {value: 'public, max-age=63072000;'};

    // Return response to viewers
    return response;
}
```