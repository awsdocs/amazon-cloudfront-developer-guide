# Add a `True-Client-IP` header to the request<a name="example-function-add-true-client-ip-header"></a>

The following example function adds a `True-Client-IP` HTTP header to the request, with the IP address of the viewer as the header’s value\. When CloudFront sends a request to an origin, the origin can determine the IP address of the CloudFront host that sent the request but not the IP address of the viewer \(client\) that sent the original request to CloudFront\. This function adds the `True-Client-IP` header so the origin can see the IP address of the viewer\.

**Important**  
To make sure that CloudFront includes this header in origin requests, you must add it to the allowed headers list in an [origin request policy](controlling-origin-requests.md)\.

This is a viewer request function\.

[See this example on GitHub](https://github.com/aws-samples/amazon-cloudfront-functions/tree/main/add-true-client-ip-header)\.

```
function handler(event) {
    var request = event.request;
    var clientIP = event.viewer.ip;

    //Add the true-client-ip header to the incoming request
    request.headers['true-client-ip'] = {value: clientIP};

    return request;
}
```