# Add a cross\-origin resource sharing \(CORS\) header to the response<a name="example-function-add-cors-header-response"></a>

The following example function adds an `Access-Control-Allow-Origin` HTTP header to the response if the response doesn’t already contain this header\. This header is part of [cross\-origin resource sharing \(CORS\)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)\. The header’s value \(`*`\) tells web browsers to allow code from any origin to access this resource\. For more information, see [Access\-Control\-Allow\-Origin](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin) on the MDN Web Docs website\.

This is a viewer response function\.

[See this example on GitHub](https://github.com/aws-samples/amazon-cloudfront-functions/tree/main/add-cors-header)\.

```
function handler(event)  {
    var response  = event.response;
    var headers  = response.headers;

    // If Access-Control-Allow-Origin CORS header is missing, add it.
    // Since JavaScript doesn't allow for hyphens in variable names, we use the dict["key"] notation.
    if (!headers['access-control-allow-origin']) {
        headers['access-control-allow-origin'] = {value: "*"};
        console.log("Access-Control-Allow-Origin was missing, adding it now.");
    }

    return response;
}
```