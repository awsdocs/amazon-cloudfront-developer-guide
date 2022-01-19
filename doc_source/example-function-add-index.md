# Add `index.html` to request URLs that don’t include a file name<a name="example-function-add-index"></a>

The following example function appends `index.html` to requests that don’t include a file name or extension in the URL\. This function can be useful for single page applications or statically generated websites that are hosted in an Amazon S3 bucket\.

This is a viewer request function\.

[See this example on GitHub](https://github.com/aws-samples/amazon-cloudfront-functions/tree/main/url-rewrite-single-page-apps)\.

```
function handler(event) {
    var request = event.request;
    var uri = request.uri;

    // Check whether the URI has a trailing slash and assume a directory with "index.html".
    if (uri.endsWith("/")) {
        request.uri += "index.html";
    } else {
        // Check whether the URI is missing a file extension.
        // This has the limitation that we can not use dots (.) in the last path segment.
        // We can use them in all other path segments without problems though.
        //
        // Examples:
        // - Correct        /some/path/file.zip     => /some/path/file.zip
        // - Correct        /some/path              => /some/path/index.html
        // - Correct:       /dir.with.dots/file.zip => /dir.with.dots/file.zip
        // - Correct:       /dir.with.dots/boom     => /dir.with.dots/boom/index.html
        // - Correct:       /dir.with.dots/         => /dir.with.dots/index.html
        // - Limitation:    /dir.with.dots          => /dir.with.dots
        //
        // To treat the last one properly we need to request it with a trailing slash.
        //
        var trailingSlash = uri.lastIndexOf("/");
        if (trailingSlash > -1 && !uri.includes(".", trailingSlash+1)) {
            request.uri += "/index.html";
        }
    }

    return request;
}
```
