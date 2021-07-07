# Tutorial: Creating a simple function with CloudFront Functions<a name="functions-tutorial"></a>

This tutorial shows you how to get started with CloudFront Functions, helping you create a simple function that redirects the viewer to a different URL, and also returns a custom response header\.

## Prerequisites<a name="functions-tutorial-prerequisites"></a>

To use CloudFront Functions, you need a CloudFront distribution\. If you don’t have one, follow the steps in [Getting started with a simple CloudFront distribution](GettingStarted.SimpleDistribution.md)\.

## Creating the function<a name="functions-tutorial-create"></a>

This procedure shows you how to use the CloudFront console to create a simple function that redirects the viewer to a different URL, and also returns a custom response header\.

**To create a function in the CloudFront console**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)\.

1. In the navigation pane, choose **Functions**\. Then choose **Create function**\.

1. Enter a function name, and then choose **Continue**\.

1. \(Optional\) For **Comment**, enter a description for the function\. For example, enter **Simple test function**\.

1. Copy the following function code, and then paste it into the code editor in the console, replacing the default code in the editor\.

   ```
   function handler(event) {
       // NOTE: This example function is for a viewer request event trigger. 
       // Choose viewer request for event trigger when you associate this function with a distribution. 
       var response = {
           statusCode: 302,
           statusDescription: 'Found',
           headers: {
               'cloudfront-functions': { value: 'generated-by-CloudFront-Functions' },
               'location': { value: 'https://aws.amazon.com/cloudfront/' }
           }
       };
       return response;
   }
   ```  
![\[Example code in the CloudFront Functions code editor.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/functions-example-code.png)

1. Choose **Save** to create a function using the example code that you pasted\. This function code redirects the viewer to a different URL, and also returns a custom response header\.  
![\[Build tab of a function page in the CloudFront console.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/functions-save.png)

   When successful, you see a banner at the top of the page that says ***Function name* saved successfully**\.
**Tip**  
You can optionally test the function before you publish it\. This tutorial doesn’t describe how to test a function, but for more information, see [Testing functions](test-function.md)\.

1. Choose the **Publish** tab, then choose the **Publish** button to publish the function\. You must publish the function before you can associate it with your CloudFront distribution\.  
![\[Publish tab of a function page in the CloudFront console.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/functions-publish.png)

   When successful, you see a banner at the top of the page that says ***Function name* published successfully**\.

1. Choose the **Associate** tab\. Then do the following:
**Warning**  
In the following steps, choose a distribution or a cache behavior that’s intended for testing\. Don’t associate this demonstration function with a distribution or cache behavior that’s used in production\.

   1. For **Distribution**, choose a distribution to associate this function with\.

   1. For **Event type**, leave the default choice \(**Viewer Request**\)\.

   1. For **Cache behavior**, choose a cache behavior to associate this function with\.

   1. Choose **Add association**\. Then, in the **Associate function to cache behavior** pop\-up window, choose **Associate**\.  
![\[Associate tab of a function page in the CloudFront console.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/functions-associate.png)

When successful, you see a banner at the top of the page that says ***Function name* associated successfully**, and the **Associated CloudFront distributions** table shows the associated distribution\. Before you verify that your function is working, wait a few minutes for the associated distribution to finish deploying\. To check the distribution’s status, choose the associated distribution, then choose **View distribution**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/functions-view-distribution.png)

When the distribution’s status is **Deployed**, you’re ready to verify that the function works\.

## Verifying the function<a name="functions-tutorial-verify"></a>

To see your function in action and verify that it works, go to your distribution’s domain name \(for example, `https://d111111abcdef8.cloudfront.net`\) in a web browser\. The function returns a redirect to the browser, so the browser automatically goes to `https://aws.amazon.com/cloudfront/`\.

If you send a request to your distribution’s domain name using a tool like curl, you see the redirect response \(`302 Found`\) and the custom response header added by the function, as emphasized in the following example\.

```
curl -v https://d111111abcdef8.cloudfront.net/
> GET / HTTP/1.1
> Host: d111111abcdef8.cloudfront.net
> User-Agent: curl/7.64.1
> Accept: */*
>
< HTTP/1.1 302 Found
< Server: CloudFront
< Date: Tue, 16 Mar 2021 18:50:48 GMT
< Content-Length: 0
< Connection: keep-alive
< Location: https://aws.amazon.com/cloudfront/
< Cloudfront-Functions: generated-by-CloudFront-Functions
< X-Cache: FunctionGeneratedResponse from cloudfront
< Via: 1.1 3035b31bddaf14eded329f8d22cf188c.cloudfront.net (CloudFront)
< X-Amz-Cf-Pop: PHX50-C2
< X-Amz-Cf-Id: ULZdIz6j43uGBlXyob_JctF9x7CCbwpNniiMlmNbmwzH1YWP9FsEHg==
```