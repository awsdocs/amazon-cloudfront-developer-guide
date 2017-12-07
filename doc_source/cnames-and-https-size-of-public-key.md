# Determining the Size of the Public Key in an SSL/TLS Certificate<a name="cnames-and-https-size-of-public-key"></a>

When you're using CloudFront alternate domain names and HTTPS, the size of the public key in an SSL/TLS certificate cannot exceed 2048 bits\. \(This is not the number of characters in the public key\.\) You can determine the size of the public key by running the following OpenSSL command:

```
openssl x509 -in path and filename of SSL/TLS certificate -text -noout 
```

where:

+ `-in` specifies the path and filename of your SSL/TLS certificate\.

+ `-text` causes OpenSSL to display the length of the public key in bits\.

+ `-noout` prevents OpenSSL from displaying the public key\.

Example output:

```
Public-Key: (2048 bit)
```