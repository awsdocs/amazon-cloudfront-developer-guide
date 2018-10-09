# Create a URL Signature Using PHP<a name="CreateURL_PHP"></a>

Any web server that runs PHP can use the PHP demo code to create policy statements and signatures for private CloudFront RTMP distributions\. The example creates a functioning web page with signed URL links that play a video stream using CloudFront streaming\. To get the example, download [Signature Code for Video Streaming in PHP](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/samples/demo-php.zip)\.

You can also create signed URLs by using the `UrlSigner` class in the AWS SDK for PHP\. For more information, see [Class UrlSigner](http://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.CloudFront.UrlSigner.html) in the *AWS SDK for PHP API Reference*\.

**Note**  
Creating a URL signature is just one part of the process of serving private content using a signed URL\. For more information about the entire process, see [Using Signed URLs](private-content-signed-urls.md)\. 

In the following code segment, the function `rsa_sha1_sign` hashes and signs the policy statement\. The arguments required are a policy statement, an out parameter to contain the signature, and the private key for your AWS account or for a trusted AWS account that you specify\. Next, the `url_safe_base64_encode` function creates a URL\-safe version of the signature\.

**Example RSA SHA1 Hashing in PHP**  <a name="RSA_SHA1_EncryptionInPHP"></a>

```
 function rsa_sha1_sign($policy, $private_key_filename) {
    $signature = "";

    // load the private key
    $fp = fopen($private_key_filename, "r");
    $priv_key = fread($fp, 8192);
    fclose($fp);
    $pkeyid = openssl_get_privatekey($priv_key);

    // compute signature
    openssl_sign($policy, $signature, $pkeyid);

    // free the key from memory
    openssl_free_key($pkeyid);

    return $signature;
 }

function url_safe_base64_encode($value) {
    $encoded = base64_encode($value);
    // replace unsafe characters +, = and / with 
    // the safe characters -, _ and ~
    return str_replace(
        array('+', '=', '/'),
        array('-', '_', '~'),
        $encoded);
 }
```

The following code constructs a *canned* policy statement needed for creating the signature\. For more information about canned policies, see [Creating a Signed URL Using a Canned Policy](private-content-creating-signed-url-canned-policy.md)\.

**Example Canned Signing Function in PHP**  <a name="CannedSigningFunctionInPHP"></a>

```
function get_canned_policy_stream_name($video_path, $private_key_filename, $key_pair_id, $expires) {
    // this policy is well known by CloudFront, but you still need to sign it, 
    // since it contains your parameters
    $canned_policy = '{"Statement":[{"Resource":"' . $video_path . '","Condition":{"DateLessThan":{"AWS:EpochTime":'. $expires . '}}}]}';
    
    // sign the canned policy
    $signature = rsa_sha1_sign($canned_policy, $private_key_filename);
    // make the signature safe to be included in a url
    $encoded_signature = url_safe_base64_encode($signature);

    // combine the above into a stream name
    $stream_name = create_stream_name($video_path, null, $encoded_signature, $key_pair_id, $expires);
    // url-encode the query string characters to work around a flash player bug
    return encode_query_params($stream_name);
    }
```

The following code constructs a *custom* policy statement needed for creating the signature\. For more information about custom policies, see [Creating a Signed URL Using a Custom Policy](private-content-creating-signed-url-custom-policy.md)\.

**Note**  
The `$expires` variable is a date/time stamp that must be an integer, not a string\.

**Example Custom Signing Function in PHP**  <a name="CustomSigningFunctionInPHP"></a>

```
function get_custom_policy_stream_name($video_path, $private_key_filename, $key_pair_id, $policy) {
    // sign the policy
    $signature = rsa_sha1_sign($policy, $private_key_filename);
    // make the signature safe to be included in a url
    $encoded_signature = url_safe_base64_encode($signature);

    // combine the above into a stream name
    $stream_name = create_stream_name($video_path, $encoded_policy, $encoded_signature, $key_pair_id, null);
    // url-encode the query string characters to work around a flash player bug
    return encode_query_params($stream_name);
    }
```

For more information about the OpenSSL implementation of SHA\-1, see [The Open Source Toolkit for SSL/TLS](http://www.openssl.org/)\.

See also
+ [Create a URL Signature Using Perl](CreateURLPerl.md)
+ [Create a URL Signature Using C\# and the \.NET Framework](CreateSignatureInCSharp.md)
+ [Create a URL Signature Using Java](CFPrivateDistJavaDevelopment.md)
+ [Tools and Code Examples for Configuring Private Content](Resources.md#resources-distributing-private-content)