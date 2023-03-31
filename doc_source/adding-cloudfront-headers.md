# Adding CloudFront request headers<a name="adding-cloudfront-headers"></a>

You can configure CloudFront to add specific HTTP headers to the requests that CloudFront receives from viewers and forwards on to your origin or [edge function](edge-functions.md)\. The values of these HTTP headers are based on characteristics of the viewer or the viewer request\. The headers provide information about the viewer's device type, IP address, geographic location, request protocol \(HTTP or HTTPS\), HTTP version, TLS connection details, and [JA3 fingerprint](https://github.com/salesforce/ja3)\.

With these headers, your origin or your edge function can receive information about the viewer without the need for you to write your own code to determine this information\. If your origin returns different responses based on the information in these headers, you can include them in the *cache key* so that CloudFront caches the responses separately\. For example, your origin might respond with content in a specific language based on the country that the viewer is in, or with content tailored to a specific device type\. Your origin might also write these headers to log files, which you can use to determine information about where your viewers are, which device types they're on, and more\.

To include these headers in the cache key, use a *cache policy*\. For more information, see [Controlling the cache key](controlling-the-cache-key.md) and [Understanding the cache key](understanding-the-cache-key.md)\.

To receive these headers at your origin but not include them in the cache key, use an *origin request policy*\. For more information, see [Controlling origin requests](controlling-origin-requests.md)\.

**Topics**
+ [Headers for determining the viewer's device type](#cloudfront-headers-device-type)
+ [Headers for determining the viewer's location](#cloudfront-headers-viewer-location)
+ [Headers for determining the viewer's header structure](#cloudfront-headers-viewer-headers)
+ [Other CloudFront headers](#cloudfront-headers-other)

## Headers for determining the viewer's device type<a name="cloudfront-headers-device-type"></a>

You can add the following headers to determine the viewer's device type\. Based on the value of the `User-Agent` header, CloudFront sets the value of these headers to `true` or `false`\. If a device falls into more than one category, more than one value can be `true`\. For example, for some tablet devices, CloudFront sets both `CloudFront-Is-Mobile-Viewer` and `CloudFront-Is-Tablet-Viewer` to `true`\.
+ `CloudFront-Is-Android-Viewer` – Set to `true` when CloudFront determines that the viewer is a device with the Android operating system\.
+ `CloudFront-Is-Desktop-Viewer` – Set to `true` when CloudFront determines that the viewer is a desktop device\.
+ `CloudFront-Is-IOS-Viewer` – Set to `true` when CloudFront determines that the viewer is a device with an Apple mobile operating system, such as iPhone, iPod touch, and some iPad devices\.
+ `CloudFront-Is-Mobile-Viewer` – Set to `true` when CloudFront determines that the viewer is a mobile device\.
+ `CloudFront-Is-SmartTV-Viewer` – Set to `true` when CloudFront determines that the viewer is a smart TV\.
+ `CloudFront-Is-Tablet-Viewer` – Set to `true` when CloudFront determines that the viewer is a tablet\.

## Headers for determining the viewer's location<a name="cloudfront-headers-viewer-location"></a>

You can add the following headers to determine the viewer's location\. CloudFront determines the values for these headers based on the viewer's IP address\. For non\-ASCII characters in these headers' values, CloudFront percent\-encodes the character according to [section 1\.2 of RFC 3986](https://tools.ietf.org/html/rfc3986#section-2.1)\.
+ `CloudFront-Viewer-Address` – Contains the IP address of the viewer and the source port of the request\. For example, a header value of `198.51.100.10:46532` means the viewer's IP address is 198\.51\.100\.10 and the request source port is 46532\.
+ `CloudFront-Viewer-ASN` – Contains the autonomous system number \(ASN\) of the viewer\.
**Note**  
`CloudFront-Viewer-Address` and `CloudFront-Viewer-ASN` can be added in an origin request policy, but not in a cache policy\.
+ `CloudFront-Viewer-Country` – Contains the two\-letter country code for the viewer's country\. For a list of country codes, see [ISO 3166\-1 alpha\-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)\.

When you add the following headers, CloudFront applies them to all requests *except* those that originate from the AWS network:
+ `CloudFront-Viewer-City` – Contains the name of the viewer's city\.
+ `CloudFront-Viewer-Country-Name` – Contains the name of the viewer's country\.
+ `CloudFront-Viewer-Country-Region` – Contains a code \(up to three characters\) that represent the viewer's region\. The region is the first\-level subdivision \(the broadest or least specific\) of the [ISO 3166\-2](https://en.wikipedia.org/wiki/ISO_3166-2) code\.
+ `CloudFront-Viewer-Country-Region-Name` – Contains the name of the viewer's region\. The region is the first\-level subdivision \(the broadest or least specific\) of the [ISO 3166\-2](https://en.wikipedia.org/wiki/ISO_3166-2) code\.
+ `CloudFront-Viewer-Latitude` – Contains the viewer's approximate latitude\.
+ `CloudFront-Viewer-Longitude` – Contains the viewer's approximate longitude\.
+ `CloudFront-Viewer-Metro-Code` – Contains the viewer's metro code\. This is present only when the viewer is in the United States\.
+ `CloudFront-Viewer-Postal-Code` – Contains the viewer's postal code\.
+ `CloudFront-Viewer-Time-Zone` Contains the viewer's time zone, in [IANA time zone database format](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) \(for example, `America/Los_Angeles`\)\.

## Headers for determining the viewer's header structure<a name="cloudfront-headers-viewer-headers"></a>

You can add the following headers to help identify the viewer based on the headers that it sends\. For example, different browsers may send HTTP headers in a certain order\. If the browser specified in the `User-Agent` header doesn’t match that browser’s expected header order, you can deny the request\. Additionally, if the `CloudFront-Viewer-Header-Count` value does not match the number of headers in `CloudFront-Viewer-Header-Order`, you can deny the request\.
+ `CloudFront-Viewer-Header-Order` – Contains the viewer's header names in the order requested, separated by a colon\. For example: `CloudFront-Viewer-Header-Order: Host:User-Agent:Accept:Accept-Encoding`\. Headers beyond the character limit of 7,680 are truncated\.
+ `CloudFront-Viewer-Header-Count` – Contains the total number of the viewer's headers\.

## Other CloudFront headers<a name="cloudfront-headers-other"></a>

You can add the following headers to determine the viewer's protocol, version, JA3 fingerprint, and TLS connection details:
+ `CloudFront-Forwarded-Proto` – Contains the protocol of the viewer's request \(HTTP or HTTPS\)\.
+ `CloudFront-Viewer-Http-Version` – Contains the HTTP version of the viewer's request\.
+ `CloudFront-Viewer-JA3-Fingerprint` – Contains the [JA3 fingerprint](https://github.com/salesforce/ja3) of the viewer\. The JA3 fingerprint can help you determine whether the request comes from a known client, whether that's malware or a malicious bot, or an expected \(allow\-listed\) application\. This header relies on the viewer's SSL/TLS `Client Hello` packet and is only present for HTTPS requests\.
**Note**  
You can add `CloudFront-Viewer-JA3-Fingerprint` in an [origin request policy](controlling-origin-requests.md), but not in a [cache policy](controlling-the-cache-key.md)\.
+ `CloudFront-Viewer-TLS` – Contains the SSL/TLS version, the cipher, and information about the SSL/TLS handshake that was used for the connection between the viewer and CloudFront\. The header value is in the following format:

  ```
  SSL/TLS_version:cipher:handshake_information
  ```

  For `handshake_information`, the header can contain the following values:
  + `fullHandshake` – A full handshake was performed for the SSL/TLS session\.
  + `sessionResumed` – A previous SSL/TLS session was resumed\.
  + `connectionReused` – A previous SSL/TLS connection was reused\.

  The following are some example values for this header:

  ```
  TLSv1.3:TLS_AES_128_GCM_SHA256:sessionResumed
  ```

  ```
  TLSv1.2:ECDHE-ECDSA-AES128-GCM-SHA256:connectionReused
  ```

  ```
  TLSv1.1:ECDHE-RSA-AES128-SHA256:fullHandshake
  ```

  ```
  TLSv1:ECDHE-RSA-AES256-SHA:fullHandshake
  ```

  For the full list of possible SSL/TLS versions and ciphers that can be in this header value, see [Supported protocols and ciphers between viewers and CloudFront](secure-connections-supported-viewer-protocols-ciphers.md)\.
**Note**  
You can add `CloudFront-Viewer-TLS` in an [origin request policy](controlling-origin-requests.md), but not in a [cache policy](controlling-the-cache-key.md)\.