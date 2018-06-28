# Caching Content Based on Cookies<a name="Cookies"></a>

For web distributions, CloudFront by default doesn't consider cookies when caching your objects in edge locations\. If your origin returns two objects and they differ only by the values in the `Set-Cookie` header, CloudFront caches only one version of the object\. \(For RTMP distributions, you cannot configure CloudFront to process cookies and CloudFront does not cache cookies in edge caches\.\)

**Important**  
Amazon S3 and some HTTP servers do not process cookies\. Do not configure CloudFront cache behaviors to forward cookies to an origin that doesn't process cookies, or you'll adversely affect cacheability and, therefore, performance\. For more information about cache behaviors, see [Cache Behavior Settings](distribution-web-values-specify.md#DownloadDistValuesCacheBehavior)\.

You can configure CloudFront to forward to your origin some or all of the cookies in viewer requests, and to cache separate versions of your objects based on cookie values in viewer requests\. CloudFront uses the cookies in viewer requests to uniquely identify an object in the cache\. 

For example, suppose that requests for `locations.html` contain a `country` cookie that has a value of either `uk` or `fr`\. When you configure CloudFront to cache your objects based on the value of the `country` cookie, CloudFront forwards requests for `locations.html` to the origin and includes the `country` cookie and cookie values\. Your origin returns `locations.html`, and CloudFront caches the object once for requests in which the value of the `country` cookie is `uk` and once for requests in which the value is `fr`\.

**Note**  
If you configure CloudFront to forward cookies to your origin, CloudFront caches based on cookie values\. This is true even if your origin ignores the cookie values in the request and, in the previous example, always returns the same version of `locations.html` to CloudFront\. As a result, CloudFront forwards more requests to your origin server for the same object, which slows performance and increases the load on your origin server\. If your origin server does not vary its response based on the value of a given cookie, we recommend that you do not configure CloudFront to forward that cookie to your origin\.

You can configure each cache behavior in a web distribution to do one of the following:
+ **Forward all cookies to your origin – ** CloudFront forwards viewer requests to your origin, including all cookies\. When your origin returns a response, CloudFront caches the response, and the cookies and cookie values in the viewer request\. \(If your origin returns cookies that were not in the viewer request, CloudFront does not cache them\.\) CloudFront returns to the viewer the requested object and all cookies and cookie values, including cookies that were not in the viewer request\.
+ **Forward a whitelist of cookies that you specify – ** CloudFront removes any cookies that aren't on the whitelist before forwarding requests to your origin\. CloudFront caches the response from your origin as well as the specified cookies and their values\. \(If your origin returns both whitelisted cookies and cookies that aren't on your whitelist, CloudFront caches only the whitelisted cookies\.\) CloudFront also returns to the viewer the object, including the specified cookies and cookie values\. If the response from the origin includes cookies that aren't on the whitelist, CloudFront returns those cookies to the viewer, too\.

  For information about specifying wildcards in cookie names, see [Whitelist Cookies \(Amazon EC2 and Other Custom Origins Only\)](distribution-web-values-specify.md#DownloadDistValuesWhitelistCookies)\.

  For the current limit on the number of cookie names that you can whitelist for each cache behavior, see [Amazon CloudFront Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_cloudfront) in the *Amazon Web Services General Reference*\. To request a higher limit, go to [https://console\.aws\.amazon\.com/support/home\#/case/create?issueType=service\-limit\-increase&limitType=service\-code\-cloudfront\-distributions](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)\.
+ **Don't forward cookies to your origin – **CloudFront doesn't cache your objects based on cookie values\. In addition, CloudFront removes the `Cookie` header from requests that it forwards to your origin and removes the `Set-Cookie` header from responses that it returns to your viewers\.

Note the following about specifying the cookies that you want to forward:

**Access Logs**  
If you configure CloudFront to log requests and to log cookies, CloudFront logs all cookies and all cookie attributes, even if you configure CloudFront not to forward cookies to your origin or if you configure CloudFront to forward only a specified list of cookies\. For more information about CloudFront logging, see [Configuring and Using Access Logs](AccessLogs.md)\.

**Case Sensitivity**  
Cookie names and values are both case sensitive\. For example, if two cookies for the same object are identical except for case, CloudFront will cache the object twice\.

**CloudFront Sorts Cookies**  
CloudFront sorts the cookies in natural order by cookie name before forwarding the request to your origin\.

**If\-Modified\-Since and If\-None\-Match**  
`If-Modified-Since` and `If-None-Match` conditional requests are not supported\.

**Suspending Caching Based on Cookies**  
If you want CloudFront to temporarily stop caching cookies and cookie attributes, configure your origin server to add the following header in responses to CloudFront:  
`no-cache="Set-Cookie"`

**Total Length of Cookie Names**  
The total number of bytes in all of the cookie names that you configure CloudFront to forward to your origin can't exceed:  
`512 – (the number of cookies that you're forwarding)`  
For example, if you configure CloudFront to forward 10 cookies to your origin, the combined length of the names of the 10 cookies can't exceed 502 bytes \(512 – 10\)\. If you configure CloudFront to forward all cookies to your origin, the length of cookie names doesn't matter\.

For information about using the CloudFront console to update a distribution so CloudFront forwards cookies to the origin, see [Viewing and Updating Distributions](HowToUpdateDistribution.md)\. For information about using the CloudFront API to update a distribution, see [PUT Distribution Config](http://docs.aws.amazon.com/cloudfront/latest/APIReference/PutConfig.html) in the *Amazon CloudFront API Reference*\.