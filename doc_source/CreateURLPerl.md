# Create a URL Signature Using Perl<a name="CreateURLPerl"></a>

The Perl script creates the signature for private content from command line arguments that specify the CloudFront URL, the path to the private key of the signer, the key ID, and an expiration date for the URL\. The tool can also decode signed URLs\. To get the tool `cfsign.pl`, go to [Amazon CloudFront Signed URLs Helper Tool](http://developer.amazonwebservices.com/connect/entry.jspa?externalID=3052&categoryID=215)\. 

**Note**  
Creating a URL signature is just one part of the process of serving private content using a signed URL\. For more information about the entire process, see [Using Signed URLs](private-content-signed-urls.md)\. 

The following example shows how you might use `cfsign.pl` to create an RTMP distribution signature\.

```
$ cfsign.pl --action encode --stream example/video.mp4 --private-key  
   /path/to/my-private-key.pem --key-pair-id PK12345EXAMPLE --expires 1265838202
```

This tool generates the policy statement from the command line arguments\. The signature that it generates is an SHA1 hash of the policy statement\. 

The following is a example base64\-encoded stream name\.

```
mp4:example/video.mp4%3FPolicy%3DewogICJTdGF0ZW1lbnQiOlt7CiAgICAgICJSZXNvdXJjZSI
6ImRyciIsCiAgICAgICJDb25kaXRpb24iOnsKICAgICAgICAiSXBBZGRyZXNzIjp7IkFXUzpTb3VyY2V
JcCI6IjAuMC4wLjAvMCJ9LAogICAgICAgICJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MjE
0NTkxNjgwMH0KICAgICAgfQogICAgEXAMPLE_%26Signature%3DewtHqEXK~68tsZt-eOFnZKGwTf2a
JlbKhXkK5SSiVqcG9pieCRV3xTEPtc29OzeXlsDvRycOM2WK0cXzcyYZhpl9tv2796ihHiCTAwIHQ8yP
17Af4nWtOLIZHoH6wkR3tU1cQHs8R1d-g-SlZGjNBXr~J2MbaJzm8i6EXAMPLE_%26Key-Pair-Id%3
DPK12345EXAMPLE
```

This signature authenticates the request to stream private content, `example/video.mp4`\. If you're using Adobe Flash Player and the stream name is passed in from a web page using JavaScript, you must base64\-encode the signature and replace characters that are invalid in a URL request parameter \(\+, =, /\) with characters that are valid \(\-, \_, and \~, respectively\)\. If the stream name is not passed in from a web page, you don't need to base64\-encode the signature\. For example, you would not base64\-encode the signature if you write your own player and the stream names are fetched from within the Adobe Flash \.swf file\.

The following example uses jwplayer with CloudFront\. 

```
<script type='text/javascript'>
  var so1 = new SWFObject
    ('http://d84l721fxaaqy9.cloudfront.net/player/player.swf',
    'mpl', '640', '360', '9');
  so1.addParam('allowfullscreen','true');
  so1.addParam('allowscriptaccess','always');
  so1.addParam('wmode','opaque');
  so1.addVariable('streamer','rtmp://s33r3xe4ayhhis.cloudfront.net/cfx/st');
  so1.addVariable("file","mp4:example/video.mp4%3FPolicy%3DewogICJTdGF0ZW1lbnQi
    Olt7CiAgICAgICJSZXNvdXJjZSI6ImRyciIsCiAgICAgICJDb25kaXRpb24iOnsKICAgICAgICA
    iSXBBZGRyZXNzIjp7IkFXUzpTb3VyY2VJcCI6IjAuMC4wLjAvMCJ9LAogICAgICAgICJEYXRlTG
    Vzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MjE0NTkxNjgwMH0KICAgICAgfQogICAgEXAMPLE_%
    26Signature%3DewtHqEXK~68tsZt-eOFnZKGwTf2aJlbKhXkK5SSiVqcG9pieCRV3xTEPtc29O
    zeXlsDvRycOM2WK0cXzcyYZhpl9tv2796ihHiCTAwIHQ8yP17Af4nWtOLIZHoH6wkR3tU1cQHs8
    R1d-g-SlZGjNBXr~J2MbaJzm8i6EXAMPLE_%26Key-Pair-Id%3DPK12345EXAMPLE
  so1.write('flv');
</script>
```

When you retrieve a stream to play from within an Adobe Flash \.swf file, do not URL\-encode the stream name, for example:

```
mp4:example/video.mp4?Policy=ewogICJTdGF0ZW1lbnQiOlt7CiAgICAgICJSZXNvdXJjZSI6ImR
yciIsCiAgICAgICJDb25kaXRpb24iOnsKICAgICAgICAiSXBBZGRyZXNzIjp7IkFXUzpTb3VyY2VJcCI
6IjAuMC4wLjAvMCJ9LAogICAgICAgICJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MjE0NTk
xNjgwMH0KICAgICAgfQogICAgEXAMPLE_&Signature=ewtHqEXK~68tsZt-eOFnZKGwTf2aJlbKhXkK
5SSiVqcG9pieCRV3xTEPtc29OzeXlsDvRycOM2WK0cXzcyYZhpl9tv2796ihHiCTAwIHQ8yP17Af4nWt
OLIZHoH6wkR3tU1cQHs8R1d-g-SlZGjNBXr~J2MbaJzm8i6EXAMPLE_&Key-Pair-Id=PK12345
EXAMPLE
```

See the comments in the Perl source code for more information about the command line switches and features of this tool\.

See also

+ [Create a URL Signature Using PHP](CreateURL_PHP.md)

+ [Create a URL Signature Using C\# and the \.NET Framework](CreateSignatureInCSharp.md)

+ [Create a URL Signature Using Java](CFPrivateDistJavaDevelopment.md)

+ [Tools and Code Examples for Configuring Private Content](Resources.md#resources-distributing-private-content)