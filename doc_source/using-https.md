# Using HTTPS with CloudFront<a name="using-https"></a>

You can configure CloudFront to require that viewers use HTTPS so that connections are encrypted when CloudFront communicates with viewers\. You also can configure CloudFront to use HTTPS with your origin so that connections are encrypted when CloudFront communicates with your origin\.

If you configure CloudFront to require HTTPS both to communicate with viewers and to communicate with your origin, here’s what happens when CloudFront receives a request:

1. A viewer submits an HTTPS request to CloudFront\. There’s some SSL/TLS negotiation here between the viewer and CloudFront\. In the end, the viewer submits the request in an encrypted format\.

1. If the CloudFront edge location contains a cached response, CloudFront encrypts the response and returns it to the viewer, and the viewer decrypts it\.

1. If the CloudFront edge location doesn’t contain a cached response, CloudFront performs SSL/TLS negotiation with your origin and, when the negotiation is complete, forwards the request to your origin in an encrypted format\.

1. Your origin decrypts the request, processes it \(generates a response\), encrypts the response, and returns the response to CloudFront\.

1. CloudFront decrypts the response, re\-encrypts it, and forwards it to the viewer\. CloudFront also caches the response in the edge location so that it’s available the next time it’s requested\.

1. The viewer decrypts the response\.

The process works basically the same way whether your origin is an Amazon S3 bucket, MediaStore, or a custom origin such as an HTTP/S server\.

**Note**  
To help thwart SSL renegotiation\-type attacks, CloudFront does not support renegotiation for viewer and origin requests\.

For information about how to require HTTPS between viewers and CloudFront, and between CloudFront and your origin, see the following topics\.

**Topics**
+ [Requiring HTTPS between viewers and CloudFront](using-https-viewers-to-cloudfront.md)
+ [Requiring HTTPS to a custom origin](using-https-cloudfront-to-custom-origin.md)
+ [Requiring HTTPS to an Amazon S3 origin](using-https-cloudfront-to-s3-origin.md)
+ [Supported protocols and ciphers between viewers and CloudFront](secure-connections-supported-viewer-protocols-ciphers.md)
+ [Supported protocols and ciphers between CloudFront and the origin](secure-connections-supported-ciphers-cloudfront-to-origin.md)
+ [Charges for HTTPS connections](ChargesForHTTPSConnections.md)