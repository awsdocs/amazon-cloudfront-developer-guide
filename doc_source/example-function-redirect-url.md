# Redirect the viewer to a new URL<a name="example-function-redirect-url"></a>

The following example function generates a response to redirect the viewer to a country\-specific URL when the request comes from within a particular country\. This function relies on the value of the `CloudFront-Viewer-Country` header to determine the viewer’s country\.

**Important**  
For this function to work, you must configure CloudFront to add the `CloudFront-Viewer-Country` header to incoming requests by adding it to the [allowed headers list in a cache policy](controlling-the-cache-key.md#cache-policy-headers)\.

## Redirect using relative path<a name="example-function-redirect-url-redirect-using-relative-path"></a>

This example redirects the viewer to a German or Polish specific URL when the viewer request comes from specific country. If the viewer request doesn’t come from Germany or Poland, the function redirects to /en/ version of the side\.

This is a **viewer request** function\.

```
function handler(event) {
            var redirects = {
                'DE': '/de/',
                'PL': '/pl/'
            };
            var headers = event.request.headers;
            var newPath = '/en/';
            var country = headers['cloudfront-viewer-country'].value;
            if (country in redirects) {
                newPath = redirects[country];
            }
            return {
                statusCode: 307,
                statusDescription: 'Temporary Redirect',
                headers: {"location": {"value": newPath}}
            };
        }
```

## Redirect using full path<a name="example-function-redirect-url-redirect-using-full-path"></a>

This example redirects the viewer to a Germany\-specific URL when the viewer request comes from Germany\. If the viewer request doesn’t come from Germany, the function returns the original, unmodified request\.

This is a **viewer request** function\.

```
function handler(event) {
    var request = event.request;
    var headers = request.headers;
    var host = request.headers.host.value;
    var country = 'DE' // Choose a country code
    var newurl = `https://${host}/de/index.html` // Change the redirect URL to your choice

    if (headers['cloudfront-viewer-country']) {
        var countryCode = headers['cloudfront-viewer-country'].value;
        if (countryCode === country) {
            var response = {
                statusCode: 302,
                statusDescription: 'Found',
                headers:
                    { "location": { "value": newurl } }
                }

            return response;
        }
    }
    return request;
}
```

## Examples on GitHub
+ [AWS Samples - redirect-based-on-country](https://github.com/aws-samples/amazon-cloudfront-functions/tree/main/redirect-based-on-country)
+ [AWS Labs - redirect-based-on-country](https://github.com/awslabs/aws-cloudfront-extensions/tree/main/function/js/redirect-based-on-country)
