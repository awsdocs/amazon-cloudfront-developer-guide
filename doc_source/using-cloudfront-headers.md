# Using the CloudFront HTTP headers<a name="using-cloudfront-headers"></a>

You can configure CloudFront to add specific HTTP headers based on characteristics of the viewer request\. With these headers, your origin can receive information about the viewer’s device type, geographic location, and more, without the need for custom code to determine this information\. If your origin returns different responses based on the information in these headers, you can include them in the *cache key* so that CloudFront caches the different responses separately\.

To receive these headers at your origin, use an *origin request policy*\. For more information, see [Controlling origin requests](controlling-origin-requests.md)\.

To include these headers in the cache key, use a *cache policy*\. For more information, see [Controlling the cache key](controlling-the-cache-key.md) and [Understanding the cache key](understanding-the-cache-key.md)\.

**Topics**
+ [Headers for determining the viewer’s device type](#cloudfront-headers-device-type)
+ [Headers for determining the viewer’s location](#cloudfront-headers-viewer-location)
+ [Other CloudFront headers](#cloudfront-headers-other)

## Headers for determining the viewer’s device type<a name="cloudfront-headers-device-type"></a>

Use the following headers to determine the viewer’s device type\. Based on the value of the `User-Agent` header, CloudFront sets the value of these headers to `true` or `false`\. If a device falls into more than one category, more than one value might be `true`\. For example, for some tablet devices, CloudFront might set both `CloudFront-Is-Mobile-Viewer` and `CloudFront-Is-Tablet-Viewer` to `true`\.
+ `CloudFront-Is-Android-Viewer` – Set to `true` when CloudFront determines that the viewer is a device with the Android operating system\.
+ `CloudFront-Is-Desktop-Viewer` – Set to `true` when CloudFront determines that the viewer is a desktop device\.
+ `CloudFront-Is-IOS-Viewer` – Set to `true` when CloudFront determines that the viewer is a device with an Apple operating system\.
+ `CloudFront-Is-Mobile-Viewer` – Set to `true` when CloudFront determines that the viewer is a mobile device\.
+ `CloudFront-Is-SmartTV-Viewer` – Set to `true` when CloudFront determines that the viewer is a smart TV\.
+ `CloudFront-Is-Tablet-Viewer` – Set to `true` when CloudFront determines that the viewer is a tablet\.

## Headers for determining the viewer’s location<a name="cloudfront-headers-viewer-location"></a>

Use the following headers to determine the viewer’s location\. CloudFront determines the values for these headers based on the viewer’s IP address\.

**Note**  
For non\-ASCII characters in these headers’ values, CloudFront percent encodes the character according to [section 1\.2 of RFC 3986](https://tools.ietf.org/html/rfc3986#section-2.1)\.
+ `CloudFront-Viewer-City` – Contains the name of the viewer’s city\.
+ `CloudFront-Viewer-Country` – Contains the two\-letter country code for the viewer’s country\. For a list of country codes, see [ISO 3166\-1 alpha\-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)\.
+ `CloudFront-Viewer-Country-Name` – Contains the name of the viewer’s country\.
+ `CloudFront-Viewer-Country-Region` – Contains a code \(up to three characters\) that represent the viewer’s region\. The region is the most specific subdivision of the [ISO 3166\-2](https://en.wikipedia.org/wiki/ISO_3166-2) code\.
+ `CloudFront-Viewer-Country-Region-Name` – Contains the name of the viewer’s region\. The region is the most specific subdivision of the [ISO 3166\-2](https://en.wikipedia.org/wiki/ISO_3166-2) code\.
+ `CloudFront-Viewer-Latitude` – Contains the viewer’s approximate latitude\.
+ `CloudFront-Viewer-Longitude` – Contains the viewer’s approximate longitude\.
+ `CloudFront-Viewer-Metro-Code` – Contains the viewer’s metro code\. This is present only when the viewer is in the United States\.
+ `CloudFront-Viewer-Postal-Code` – Contains the viewer’s postal code\.
+ `CloudFront-Viewer-Time-Zone` Contains the viewer’s time zone, in [IANA time zone database format](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) \(for example, `America/Los_Angeles`\)\.

## Other CloudFront headers<a name="cloudfront-headers-other"></a>

Use the following headers to determine the protocol and HTTP version of the viewer’s request\.
+ `CloudFront-Forwarded-Proto` – Contains the protocol of the viewer’s request \(HTTP or HTTPS\)\.
+ `CloudFront-Viewer-Http-Version` – Contains the HTTP version of the viewer’s request\.