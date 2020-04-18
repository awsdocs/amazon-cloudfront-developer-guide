# Testing a Distribution<a name="distribution-web-testing"></a>

After you've created your distribution, CloudFront knows where your origin server is, and you know the domain name associated with the distribution\. You can create links to your objects using the CloudFront domain name, and CloudFront will serve the objects to your webpage or application\.

**Note**  
You must wait until the status of the distribution changes to **Deployed** before you can test your links\.<a name="distribution-web-testing-procedure"></a>

**To create links to objects in a web distribution**

1. Copy the following HTML code into a new file, replace *domain\-name* with your distribution's domain name, and replace *object\-name* with the name of your object\.

   ```
   <html>
   <head>My CloudFront Test</head>
   <body>
   <p>My text content goes here.</p>
   <p><img src="http://domain-name/object-name" alt="my test image"
   </body>
   </html>
   ```

   For example, if your domain name were `d111111abcdef8.cloudfront.net` and your object were `image.jpg`, the URL for the link would be:

   `http://d111111abcdef8.cloudfront.net/image.jpg`\.

   If your object is in a folder on your origin server, then the folder must also be included in the URL\. For example, if image\.jpg were located in the images folder on your origin server, then the URL would be: 

   `http://d111111abcdef8.cloudfront.net/images/image.jpg`

1. Save the HTML code in a file that has an \.html file name extension\.

1. Open your webpage in a browser to ensure that you can see your object\.

The browser returns your page with the embedded image file, served from the edge location that CloudFront determined was appropriate to serve the object\.