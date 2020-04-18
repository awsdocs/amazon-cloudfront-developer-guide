# Determining the Size of the Public Key in an SSL/TLS Certificate<a name="cnames-and-https-size-of-public-key"></a>

When you're using CloudFront alternate domain names and HTTPS, the maximum size of the public key in an SSL/TLS certificate is 2048 bits\. \(This is the key size, not the number of characters in the public key\.\) If you use AWS Certificate Manager for your certificates, although ACM supports larger keys, you cannot use the larger keys with CloudFront\. 

You can determine the size of the public key by running the following OpenSSL command:

```
openssl x509 -in path and filename of SSL/TLS certificate -text -noout 
```

Where:
+ `-in` specifies the path and file name of your SSL/TLS certificate\.
+ `-text` causes OpenSSL to display the length of the public key in bits\.
+ `-noout` prevents OpenSSL from displaying the public key\.

Example output:

```
Public-Key: (2048 bit)
```