# Add `index.html` to request URLs that don’t include a file name<a name="example-function-add-index"></a>

The following example function appends `index.html` to requests that don’t include a file name or extension in the URL\. This function can be useful for single page applications or statically generated websites that are hosted in an Amazon S3 bucket\.

This is a viewer request function\.

[See this example on GitHub](https://github.com/aws-samples/amazon-cloudfront-functions/tree/main/url-rewrite-single-page-apps)\.

```
function handler(event) {
    var request = event.request;
    var uri = request.uri;

    // Check whether the URI is missing a file name.
    if (uri.endsWith('/')) {
        request.uri += 'index.html';
    }
    // Check whether the URI is missing a file extension.
    else if (!uri.includes('.')) {
        request.uri += '/index.html';
    }

    return request;
}
```