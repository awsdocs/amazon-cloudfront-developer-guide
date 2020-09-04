# Using HTTPS with CloudFront<a name="using-https"></a>

For web distributions, you can configure CloudFront to require that viewers use HTTPS to request your objects, so that connections are encrypted when CloudFront communicates with viewers\. You also can configure CloudFront to use HTTPS to get objects from your origin, so that connections are encrypted when CloudFront communicates with your origin\.

If you configure CloudFront to require HTTPS both to communicate with viewers and to communicate with your origin, here's what happens when CloudFront receives a request for an object:

1. A viewer submits an HTTPS request to CloudFront\. There's some SSL/TLS negotiation here between the viewer and CloudFront\. In the end, the viewer submits the request in an encrypted format\.

1. If the object is in the CloudFront edge cache, CloudFront encrypts the response and returns it to the viewer, and the viewer decrypts it\.

1. If the object is not in the CloudFront cache, CloudFront performs SSL/TLS negotiation with your origin and, when the negotiation is complete, forwards the request to your origin in an encrypted format\.

1. Your origin decrypts the request, encrypts the requested object, and returns the object to CloudFront\.

1. CloudFront decrypts the response, re\-encrypts it, and forwards the object to the viewer\. CloudFront also saves the object in the edge cache so that the object is available the next time it's requested\.

1. The viewer decrypts the response\.

The process works basically the same way whether your origin is an Amazon S3 bucket, MediaStore, or a custom origin such as an HTTP/S server\.

**Note**  
To help thwart SSL renegotiation\-type attacks, CloudFront does not support renegotiation for viewer and origin requests\.

For information about how to require HTTPS between viewers and CloudFront, and between CloudFront and your origin, see the following topics\.

**Topics**
+ [Requiring HTTPS for Communication Between Viewers and CloudFront](using-https-viewers-to-cloudfront.md)
+ [Requiring HTTPS for Communication Between CloudFront and Your Custom Origin](using-https-cloudfront-to-custom-origin.md)
+ [Requiring HTTPS for Communication Between CloudFront and Your Amazon S3 Origin](using-https-cloudfront-to-s3-origin.md)
+ [Supported protocols and ciphers between viewers and CloudFront](secure-connections-supported-viewer-protocols-ciphers.md)
+ [Supported protocols and ciphers between CloudFront and the origin](secure-connections-supported-ciphers-cloudfront-to-origin.md)
+ [Charges for HTTPS connections](ChargesForHTTPSConnections.md)