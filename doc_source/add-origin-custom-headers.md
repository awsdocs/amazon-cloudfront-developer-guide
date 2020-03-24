# Adding Custom Headers to Origin Requests<a name="add-origin-custom-headers"></a>

You can configure CloudFront to add custom headers to the requests that it sends to your origin\. These custom headers enable you to send and gather information from your origin that you don’t get with typical viewer requests\. These headers can even be customized for each origin\. CloudFront supports custom headers for both for custom and Amazon S3 origins\.

**Note**  
Adding custom headers applies only to web distributions\.

**Topics**
+ [Use Cases for Origin Custom Headers](#add-origin-custom-headers-use-cases)
+ [Configuring CloudFront to Add Custom Headers to Origin Requests](#add-origin-custom-headers-configure)
+ [Custom Headers that CloudFront Can’t Add to Origin Requests](#add-origin-custom-headers-blacklist)
+ [Configure CloudFront to Forward Authorization Headers](#add-origin-custom-headers-forward-authorization)

## Use Cases for Origin Custom Headers<a name="add-origin-custom-headers-use-cases"></a>

You can use custom headers for a variety of things, such as the following:

**Identifying requests from CloudFront**  
You can identify the requests that your origin receives from CloudFront\. This can be useful if you want to know if users are bypassing CloudFront, or if you’re using more than one CDN and you want information about which requests are coming from each CDN\.  
If you’re using an Amazon S3 origin and you enable [Amazon S3 server access logging](https://docs.aws.amazon.com/AmazonS3/latest/dev/ServerLogs.html), the logs don’t include header information\.

**Determining which requests come from a particular distribution**  
If you configure more than one CloudFront distribution to use the same origin, you can add different custom headers in each distribution\. You can then use the logs from your origin to determine which requests came from which CloudFront distribution\.

**Enabling cross\-origin resource sharing \(CORS\)**  
If some of your viewers don’t support cross\-origin resource sharing \(CORS\), you can configure CloudFront to always add the `Origin` header to requests that it sends to your origin\. Then you can configure your origin to return the `Access-Control-Allow-Origin` header for every request\. You must also [configure CloudFront to respect CORS settings](header-caching.md#header-caching-web-cors)\.

**Controlling access to content**  
You can use custom headers to control access to content\. By configuring your origin to respond to requests only when they include a custom header that gets added by CloudFront, you prevent users from bypassing CloudFront and accessing your content directly on the origin\. For more information, see [ Restricting Access to Files on Custom Origins](private-content-overview.md#forward-custom-headers-restrict-access)\.

## Configuring CloudFront to Add Custom Headers to Origin Requests<a name="add-origin-custom-headers-configure"></a>

To configure a web distribution to add custom headers to requests that it sends to your origin, update the origin configuration using one of the following methods:
+ **CloudFront console** – When you create or update a distribution, specify header names and values in the **Origin Custom Headers** settings\. For more information, see [Creating a Distribution](distribution-web-creating-console.md) or [Updating a Distribution](HowToUpdateDistribution.md)\.
+ **CloudFront API** – For each origin that you want to add custom headers to, specify the header names and values in the `CustomHeaders` field inside `Origin`\. For more information, see [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html) or [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)\.

If the header names and values that you specify are not already present in the viewer request, CloudFront adds them to the origin request\. If a header is present, CloudFront overwrites the header value before forwarding the request to the origin\.

For the quotas \(formerly known as limits\) that apply to origin custom headers, see [Quotas on Custom Headers \(Web Distributions Only\)](cloudfront-limits.md#limits-custom-headers)\.

## Custom Headers that CloudFront Can’t Add to Origin Requests<a name="add-origin-custom-headers-blacklist"></a>

You can’t configure CloudFront to add any of the following headers to requests that it sends to your origin:
+ `Cache-Control`
+ `Connection`
+ `Content-Length`
+ `Cookie`
+ `Host`
+ `If-Match`
+ `If-Modified-Since`
+ `If-None-Match`
+ `If-Range`
+ `If-Unmodified-Since`
+ `Max-Forwards`
+ `Pragma`
+ `Proxy-Authorization`
+ `Proxy-Connection`
+ `Range`
+ `Request-Range`
+ `TE`
+ `Trailer`
+ `Transfer-Encoding`
+ `Upgrade`
+ `Via`
+ Headers that begin with `X-Amz-`
+ Headers that begin with `X-Edge-`
+ `X-Real-Ip`

## Configure CloudFront to Forward Authorization Headers<a name="add-origin-custom-headers-forward-authorization"></a>

When it forwards requests to your origin, CloudFront removes some viewer headers by default, including authorization headers\. To always forward authorization headers for a specific cache behavior, whitelist the headers for that cache behavior\. To learn more, see [Whitelist Headers](distribution-web-values-specify.md#DownloadDistValuesWhitelistHeaders)\.