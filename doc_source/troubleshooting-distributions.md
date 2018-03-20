# Troubleshooting Distribution Issues<a name="troubleshooting-distributions"></a>

Use the information here to help you diagnose and fix access\-denied issues or other common issues that you might encounter when working with Amazon CloudFront distributions\.

## I Can't View the Files in My Web Distribution<a name="troubleshooting-web-distribution"></a>

If you can't view the files in your CloudFront web distribution, see the following topics for some common solutions\.

### Did You Sign Up for Both CloudFront and Amazon S3?<a name="Troubleshooting.SigningUp"></a>

To use Amazon CloudFront with an Amazon S3 origin, you must sign up for both CloudFront and Amazon S3, separately\. For more information about signing up for CloudFront and Amazon S3, see [Getting Started with CloudFront](GettingStarted.md)\.

### Are Your Amazon S3 Bucket and Object Permissions Set Correctly?<a name="Troubleshooting.Permissions"></a>

If you are using CloudFront with an Amazon S3 origin, the original versions of your content are stored in an Amazon S3 bucket\. The easiest way to use CloudFront with Amazon S3 is to make all your objects publicly readable in Amazon S3\. To do this, you must explicitly enable public read privileges for each object that you upload to Amazon S3\.

If your content is not publicly readable, you must create a CloudFront origin access identity so that CloudFront can access it\. For more information about CloudFront origin access identities, see [Using an Origin Access Identity to Restrict Access to Your Amazon S3 Content](private-content-restricting-access-to-s3.md)\.

Object properties and bucket properties are independent\. You must explicitly grant privileges to each object in Amazon S3\. Objects do not inherit properties from buckets, and object properties must be set independently of the bucket\.

### Is Your Alternate Domain Name \(CNAME\) Correctly Configured?<a name="Troubleshooting.CNAME"></a>

If you already have an existing CNAME record for your domain name, update that record or replace it with a new one that points to your distribution's domain name\.

Also, make sure that your CNAME record points to your distribution's domain name, not your Amazon S3 bucket\. You can confirm that the CNAME record in your DNS system points to your distribution's domain name\. To do so, use a DNS tool like dig\. For information about dig, see [http://www\.kloth\.net/services/dig\.php](http://www.kloth.net/services/dig.php)\.

The following example shows a dig request for a domain name called `images.example.com` and the relevant part of the response\. Under `ANSWER SECTION`, see the line that contains `CNAME`\. The CNAME record for your domain name is set up correctly if the value on the right side of CNAME is your CloudFront distribution's domain name\. If it's your Amazon S3 origin server bucket or some other domain name, then the CNAME record is set up incorrectly\.

```
 1. 	[prompt]>
 2.  dig images.example.com
 3. 	
 4. 	; <<> DiG 9.3.3rc2 <<> images.example.com
 5. 	;; global options:	printcmd
 6. 	;; Got answer:
 7. 	;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 15917
 8. 	;; flags: qr rd ra; QUERY: 1, ANSWER: 9, AUTHORITY: 2, ADDITIONAL: 0
 9. 	;; QUESTION SECTION:
10. 	;images.example.com.		 IN		A
11. 	;; ANSWER SECTION:
12. 	images.example.com. 10800 IN	CNAME	d111111abcdef8.cloudfront.net.
13. 	...
14. 	...
```

For more information about CNAMEs, see [Adding and Moving Alternate Domain Names \(CNAMEs\)](CNAMEs.md)\.

### Are You Referencing the Correct URL for Your CloudFront Distribution?<a name="Troubleshooting.URL"></a>

Make sure that the URL that you're referencing uses the domain name \(or CNAME\) of your CloudFront distribution, not your Amazon S3 bucket or custom origin\. 

### Do You Need Help Troubleshooting a Custom Origin?<a name="Troubleshooting.CustomOrigin"></a>

If you need AWS to help you troubleshoot a custom origin, we probably will need to inspect the `X-Amz-Cf-Id` header entries from your requests\. If you are not already logging these entries, you might want to consider it for the future\. For more information, see [Requirements and Recommendations for Using Amazon EC2 and Other Custom Origins](CustomOriginBestPractices.md)\. For further help, see the [AWS Support Center](https://console.aws.amazon.com/support/home#/)\.

## I Can't View the Files in My RTMP Distribution<a name="Troubleshooting.Configuration"></a>

If you can't view the files in your RTMP distribution, are your URL and your playback client correctly configured? RTMP distributions require you to use an RTMP protocol instead of HTTP, and you must make a few minor configuration changes to your playback client\. For information about creating RTMP distributions, see [Task List for Streaming Media Files Using RTMP](distribution-rtmp-creating.md)\. 

## Error Message: Certificate: <certificate\-id> Is Being Used by CloudFront<a name="troubleshooting-certificate-error"></a>

**Problem:** You're trying to delete an SSL/TLS certificate from the IAM certificate store, and you're getting the message "Certificate: <certificate\-id> is being used by CloudFront\."

**Solution:** Every CloudFront web distribution must be associated either with the default CloudFront certificate or with a custom SSL/TLS certificate\. Before you can delete an SSL/TLS certificate, you must either rotate the certificate \(replace the current custom SSL/TLS certificate with another custom SSL/TLS certificate\) or revert from using a custom SSL/TLS certificate to using the default CloudFront certificate\. To do that, perform the procedure in the applicable section:
+ [Rotating SSL/TLS Certificates](cnames-and-https-rotate-certificates.md)
+ [Reverting from a Custom SSL/TLS Certificate to the Default CloudFront Certificate](cnames-and-https-revert-to-cf-certificate.md)