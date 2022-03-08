# Caching content based on query string parameters<a name="QueryStringParameters"></a>

Some web applications use query strings to send information to the origin\. A query string is the part of a web request that appears after a `?` character; the string can contain one or more parameters, separated by `&` characters\. In the following example, the query string includes two parameters, *color=red* and *size=large*:

`https://d111111abcdef8.cloudfront.net/images/image.jpg?color=red&size=large`

For distributions, you can choose if you want CloudFront to forward query strings to your origin and whether to cache your content based on all parameters or on selected parameters\. Why might this be useful? Consider the following example\.

Suppose that your website is available in five languages\. The directory structure and file names for all five versions of the website are identical\. As a user views your website, requests that are forwarded to CloudFront include a language query string parameter based on the language that the user chose\. You can configure CloudFront to forward query strings to the origin and to cache based on the language parameter\. If you configure your web server to return the version of a given page that corresponds with the selected language, CloudFront caches each language version separately, based on the value of the language query string parameter\.

In this example, if the main page for your website is `main.html`, the following five requests cause CloudFront to cache `main.html` five times, once for each value of the language query string parameter:
+ `https://d111111abcdef8.cloudfront.net/main.html?language=de`
+ `https://d111111abcdef8.cloudfront.net/main.html?language=en`
+ `https://d111111abcdef8.cloudfront.net/main.html?language=es`
+ `https://d111111abcdef8.cloudfront.net/main.html?language=fr`
+ `https://d111111abcdef8.cloudfront.net/main.html?language=jp`

Note the following:
+ Some HTTP servers don't process query string parameters and, therefore, don't return different versions of an object based on parameter values\. For these origins, if you configure CloudFront to forward query string parameters to the origin, CloudFront still caches based on the parameter values even though the origin returns identical versions of the object to CloudFront for every parameter value\.
+ For query string parameters to work as described in the example above with the languages, you must use the `&` character as the delimiter between query string parameters\. If you use a different delimiter, you may get unexpected results, depending on which parameters you specify for CloudFront to use as a basis for caching, and the order in which the parameters appear in the query string\.

  The following examples show what happens if you use a different delimiter and you configure CloudFront to cache based only on the `color` parameter: 
  + In the following request, CloudFront caches your content based on the value of the `color` parameter, but CloudFront interprets the value as *red;size=large*:

    `https://d111111abcdef8.cloudfront.net/images/image.jpg?color=red;size=large`
  + In the following request, CloudFront caches your content but doesn't base caching on the query string parameters\. This is because you configured CloudFront to cache based on the `color` parameter, but CloudFront interprets the following string as containing only a `size` parameter that has a value of *large;color=red*:

    `https://d111111abcdef8.cloudfront.net/images/image.jpg?size=large;color=red`

You can configure CloudFront to do one of the following:
+ Don't forward query strings to the origin at all\. If you don't forward query strings, CloudFront doesn't cache based on query string parameters\.
+ Forward query strings to the origin, and cache based on all parameters in the query string\.
+ Forward query strings to the origin, and cache based on specified parameters in the query string\.

For more information, see [Optimizing caching](#query-string-parameters-optimizing-caching)\.

**Topics**
+ [Console and API settings for query string forwarding and caching](#query-string-parameters-console)
+ [Optimizing caching](#query-string-parameters-optimizing-caching)
+ [Query string parameters and CloudFront standard logs \(access logs\)](#query-string-parameters-access-logs)

## Console and API settings for query string forwarding and caching<a name="query-string-parameters-console"></a>

To configure query string forwarding and caching in the CloudFront console, see the following settings in [Values that you specify when you create or update a distribution](distribution-web-values-specify.md):
+ [Query string forwarding and caching](distribution-web-values-specify.md#DownloadDistValuesQueryString)
+ [Query string whitelist](distribution-web-values-specify.md#DownloadDistValuesQueryStringWhiteList)

To configure query string forwarding and caching with the CloudFront API, see the following settings in [DistributionConfig](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_DistributionConfig.html) and in [DistributionConfigWithTags](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_DistributionConfigWithTags.html) in the *Amazon CloudFront API Reference*:
+ `QueryString`
+ `QueryStringCacheKeys`

## Optimizing caching<a name="query-string-parameters-optimizing-caching"></a>

When you configure CloudFront to cache based on query string parameters, you can take the following steps to reduce the number of requests that CloudFront forwards to your origin\. When CloudFront edge locations serve objects, you reduce the load on your origin server and reduce latency because objects are served from locations that are closer to your users\.

**Cache based only on parameters for which your origin returns different versions of an object**  
For each query string parameter that your web application forwards to CloudFront, CloudFront forwards requests to your origin for every parameter value and caches a separate version of the object for every parameter value\. This is true even if your origin always returns the same object regardless of the parameter value\. For multiple parameters, the number of requests and the number of objects multiply\. For example, if requests for an object include two parameters that each have three different values, CloudFront caches six versions of that object, assuming you follow the other recommendations in this section\.  
We recommend that you configure CloudFront to cache based only on the query string parameters for which your origin returns different versions, and that you carefully consider the merits of caching based on each parameter\. For example, suppose you have a retail website\. You have pictures of a jacket in six different colors, and the jacket comes in 10 different sizes\. The pictures that you have of the jacket show the different colors but not the different sizes\. To optimize caching, you should configure CloudFront to cache based only on the color parameter, not on the size parameter\. This increases the likelihood that CloudFront can serve a request from the cache, which improves performance and reduces the load on your origin\.

**Always list parameters in the same order**  
The order of parameters matters in query strings\. In the following example, the query strings are identical except that the parameters are in a different order\. This causes CloudFront to forward two separate requests for image\.jpg to your origin and to cache two separate versions of the object:  
+ `https://d111111abcdef8.cloudfront.net/images/image.jpg?color=red&size=large`
+ `https://d111111abcdef8.cloudfront.net/images/image.jpg?size=large&color=red`
We recommend that you always list parameter names in the same order, such as alphabetical order\.

**Always use the same case for parameter names and values**  
CloudFront considers the case of parameter names and values when caching based on query string parameters\. In the following example, the query strings are identical except for the case of parameter names and values\. This causes CloudFront to forward four separate requests for image\.jpg to your origin and to cache four separate versions of the object:  
+ `https://d111111abcdef8.cloudfront.net/images/image.jpg?color=red`
+ `https://d111111abcdef8.cloudfront.net/images/image.jpg?color=Red`
+ `https://d111111abcdef8.cloudfront.net/images/image.jpg?Color=red`
+ `https://d111111abcdef8.cloudfront.net/images/image.jpg?Color=Red`
We recommend that you use case consistently for parameter names and values, such as all lowercase\.

**Don’t use parameter names that conflict with signed URLs**  
If you're using signed URLs to restrict access to your content \(if you added trusted signers to your distribution\), CloudFront removes the following query string parameters before forwarding the rest of the URL to your origin:  
+ `Expires`
+ `Key-Pair-Id`
+ `Policy`
+ `Signature`
If you're using signed URLs and you want to configure CloudFront to forward query strings to your origin, your own query string parameters cannot be named `Expires`, `Key-Pair-Id`, `Policy`, or `Signature`\.

## Query string parameters and CloudFront standard logs \(access logs\)<a name="query-string-parameters-access-logs"></a>

If you enable logging, CloudFront logs the full URL, including query string parameters\. This is true regardless of whether you have configured CloudFront to forward query strings to the origin\. For more information about CloudFront logging, see [Configuring and using standard logs \(access logs\)](AccessLogs.md)\.