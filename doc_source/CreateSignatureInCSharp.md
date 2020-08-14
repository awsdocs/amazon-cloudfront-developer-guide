# Create a URL Signature Using C\# and the \.NET Framework<a name="CreateSignatureInCSharp"></a>

The C\# examples in this section implement an example application that demonstrates how to create the signatures for CloudFront private distributions using canned and custom policy statements\. The examples includes utility functions based on the [AWS \.NET SDK](http://aws.amazon.com/sdkfornet/) that can be useful in \.NET applications\. 

You can also create signed URLs and signed cookies by using the AWS SDK for \.NET\. In the [AWS SDK for \.NET API Reference](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/), see the following topics:
+ **Signed URLs** – Amazon\.CloudFront > AmazonCloudFrontUrlSigner
+ **Signed cookies** – Amazon\.CloudFront > AmazonCloudFrontCookieSigner

**Note**  
Creating a URL signature is just one part of the process of serving private content using a signed URL\. For more information about the entire process, see [Using Signed URLs](private-content-signed-urls.md)\.

To download the code, go to [Signature Code in C\#](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/samples/AWS_PrivateCF_Distributions.zip)\.

To use the RSA keys provided by [AWS Account/Security](http://aws-portal.amazon.com/gp/aws/developer/account/index.html?action=access-key) in the \.NET framework, you must convert the AWS\-supplied \.pem files to the XML format that the \.NET framework uses\.  

After conversion, the RSA private key file is in the following format:

**Example RSA Private Key in the XML \.NET Framework Format**  <a name="RSAPrivateKeyXML.NETFrameworkFormat"></a>

```
<RSAKeyValue>
  <Modulus>
    wO5IvYCP5UcoCKDo1dcspoMehWBZcyfs9QEzGi6Oe5y+ewGr1oW+vB2GPB
    ANBiVPcUHTFWhwaIBd3oglmF0lGQljP/jOfmXHUK2kUUnLnJp+oOBL2NiuFtqcW6h/L5lIpD8Yq+NRHg
    Ty4zDsyr2880MvXv88yEFURCkqEXAMPLE=
  </Modulus>
  <Exponent>AQAB</Exponent>
  <P>
    5bmKDaTz
    npENGVqz4Cea8XPH+sxt+2VaAwYnsarVUoSBeVt8WLloVuZGG9IZYmH5KteXEu7fZveYd9UEXAMPLE==
  </P>
  <Q>
    1v9l/WN1a1N3rOK4VGoCokx7kR2SyTMSbZgF9IWJNOugR/WZw7HTnjipO3c9dy1Ms9pUKwUF4
    6d7049EXAMPLE==
  </Q>
  <DP>
    RgrSKuLWXMyBH+/l1Dx/I4tXuAJIrlPyo+VmiOc7b5NzHptkSHEPfR9s1
    OK0VqjknclqCJ3Ig86OMEtEXAMPLE==
  </DP>
  <DQ>
    pjPjvSFw+RoaTu0pgCA/jwW/FGyfN6iim1RFbkT4
    z49DZb2IM885f3vf35eLTaEYRYUHQgZtChNEV0TEXAMPLE==
  </DQ>
  <InverseQ>
    nkvOJTg5QtGNgWb9i
    cVtzrL/1pFEOHbJXwEJdU99N+7sMK+1066DL/HSBUCD63qD4USpnf0myc24in0EXAMPLE==</InverseQ>
  <D>
      Bc7mp7XYHynuPZxChjWNJZIq+A73gm0ASDv6At7F8Vi9r0xUlQe/v0AQS3ycN8QlyR4XMbzMLYk
      3yjxFDXo4ZKQtOGzLGteCU2srANiLv26/imXA8FVidZftTAtLviWQZBVPTeYIA69ATUYPEq0a5u5wjGy
      UOij9OWyuEXAMPLE=
   </D>
</RSAKeyValue>
```

The following C\# code creates a signed URL that uses a canned policy by doing the following:
+ Creates a policy statement\.
+ Hashes the policy statement using SHA1, and signs the result using RSA and the private key for your AWS account or for a trusted AWS account that you specify\.
+ Base64\-encodes the hashed and signed policy statement and replaces special characters to make the string safe to use as a URL request parameter\.
+ Concatenates the values\.

For the complete implementation, see the example at [Signature Code in C\#](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/samples/AWS_PrivateCF_Distributions.zip)\. 

**Example Canned Policy Signing Method in C\#**  <a name="ExampleCannedPolicySigningMethod-CSharp"></a>

```
public static string ToUrlSafeBase64String(byte[] bytes)
{
    return System.Convert.ToBase64String(bytes)
        .Replace('+', '-')
        .Replace('=', '_')
        .Replace('/', '~');
}

public static string CreateCannedPrivateURL(string urlString, 
    string durationUnits, string durationNumber, string pathToPolicyStmnt, 
    string pathToPrivateKey, string privateKeyId)
{
    // args[] 0-thisMethod, 1-resourceUrl, 2-seconds-minutes-hours-days 
    // to expiration, 3-numberOfPreviousUnits, 4-pathToPolicyStmnt, 
    // 5-pathToPrivateKey, 6-PrivateKeyId

    TimeSpan timeSpanInterval = GetDuration(durationUnits, durationNumber);

    // Create the policy statement.
    string strPolicy = CreatePolicyStatement(pathToPolicyStmnt,
        urlString, 
        DateTime.Now, 
        DateTime.Now.Add(timeSpanInterval), 
        "0.0.0.0/0");
    if ("Error!" == strPolicy) return "Invalid time frame." + 
        "Start time cannot be greater than end time.";

    // Copy the expiration time defined by policy statement.
    string strExpiration = CopyExpirationTimeFromPolicy(strPolicy);

    // Read the policy into a byte buffer.
    byte[] bufferPolicy = Encoding.ASCII.GetBytes(strPolicy);

    // Initialize the SHA1CryptoServiceProvider object and hash the policy data.
    using (SHA1CryptoServiceProvider 
        cryptoSHA1 = new SHA1CryptoServiceProvider())
    {
        bufferPolicy = cryptoSHA1.ComputeHash(bufferPolicy);

        // Initialize the RSACryptoServiceProvider object.
        RSACryptoServiceProvider providerRSA = new RSACryptoServiceProvider();
        XmlDocument xmlPrivateKey = new XmlDocument();

        // Load PrivateKey.xml, which you created by converting your 
        // .pem file to the XML format that the .NET framework uses.  
        // Several tools are available. 
        xmlPrivateKey.Load(pathToPrivateKey);

        // Format the RSACryptoServiceProvider providerRSA and 
        // create the signature.
        providerRSA.FromXmlString(xmlPrivateKey.InnerXml);
        RSAPKCS1SignatureFormatter rsaFormatter = 
            new RSAPKCS1SignatureFormatter(providerRSA);
        rsaFormatter.SetHashAlgorithm("SHA1");
        byte[] signedPolicyHash = rsaFormatter.CreateSignature(bufferPolicy);

        // Convert the signed policy to URL-safe base64 encoding and 
        // replace unsafe characters + = / with the safe characters - _ ~
        string strSignedPolicy = ToUrlSafeBase64String(signedPolicyHash);

        // Concatenate the URL, the timestamp, the signature, 
        // and the key pair ID to form the signed URL.
        return urlString + 
            "?Expires=" + 
            strExpiration + 
            "&Signature=" + 
            strSignedPolicy + 
            "&Key-Pair-Id=" + 
            privateKeyId;
    }
}
```

The following C\# code creates a signed URL that uses a custom policy by doing the following:

1. Creates a policy statement\.

1. Base64\-encodes the policy statement and replaces special characters to make the string safe to use as a URL request parameter\.

1. Hashes the policy statement using SHA1, and encrypts the result using RSA and the private key for your AWS account or for a trusted AWS account that you specify\.

1. Base64\-encodes the hashed policy statement and replacing special characters to make the string safe to use as a URL request parameter\.

1. Concatenates the values\.

For the complete implementation, see the example at [Signature Code in C\#](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/samples/AWS_PrivateCF_Distributions.zip)\. 

**Example Custom Policy Signing Method in C\#**  <a name="ExampleCustomPolicySigningMethod-CSharp"></a>

```
public static string ToUrlSafeBase64String(byte[] bytes)
{
    return System.Convert.ToBase64String(bytes)
        .Replace('+', '-')
        .Replace('=', '_')
        .Replace('/', '~');
}

public static string CreateCustomPrivateURL(string urlString, 
    string durationUnits, string durationNumber, string startIntervalFromNow, 
    string ipaddress, string pathToPolicyStmnt, string pathToPrivateKey, 
    string PrivateKeyId)
{
    // args[] 0-thisMethod, 1-resourceUrl, 2-seconds-minutes-hours-days 
    // to expiration, 3-numberOfPreviousUnits, 4-starttimeFromNow, 
    // 5-ip_address, 6-pathToPolicyStmt, 7-pathToPrivateKey, 8-privateKeyId

    TimeSpan timeSpanInterval = GetDuration(durationUnits, durationNumber);
    TimeSpan timeSpanToStart = GetDurationByUnits(durationUnits, 
        startIntervalFromNow);
    if (null == timeSpanToStart) 
        return "Invalid duration units." + 
            "Valid options: seconds, minutes, hours, or days";
            
    string strPolicy = CreatePolicyStatement(
        pathToPolicyStmnt, urlString, DateTime.Now.Add(timeSpanToStart), 
        DateTime.Now.Add(timeSpanInterval), ipaddress);

    // Read the policy into a byte buffer.
    byte[] bufferPolicy = Encoding.ASCII.GetBytes(strPolicy);

    // Convert the policy statement to URL-safe base64 encoding and 
    // replace unsafe characters + = / with the safe characters - _ ~

    string urlSafePolicy = ToUrlSafeBase64String(bufferPolicy);

    // Initialize the SHA1CryptoServiceProvider object and hash the policy data.
    byte[] bufferPolicyHash;
    using (SHA1CryptoServiceProvider cryptoSHA1 = 
        new SHA1CryptoServiceProvider())
    {
        bufferPolicyHash = cryptoSHA1.ComputeHash(bufferPolicy);

        // Initialize the RSACryptoServiceProvider object.
        RSACryptoServiceProvider providerRSA = new RSACryptoServiceProvider();
        XmlDocument xmlPrivateKey = new XmlDocument();

        // Load PrivateKey.xml, which you created by converting your 
        // .pem file to the XML format that the .NET framework uses.  
        // Several tools are available. 
        xmlPrivateKey.Load("PrivateKey.xml");

        // Format the RSACryptoServiceProvider providerRSA 
        // and create the signature.
        providerRSA.FromXmlString(xmlPrivateKey.InnerXml);
        RSAPKCS1SignatureFormatter RSAFormatter = 
            new RSAPKCS1SignatureFormatter(providerRSA);
        RSAFormatter.SetHashAlgorithm("SHA1");
        byte[] signedHash = RSAFormatter.CreateSignature(bufferPolicyHash);

        // Convert the signed policy to URL-safe base64 encoding and 
        // replace unsafe characters + = / with the safe characters - _ ~
        string strSignedPolicy = ToUrlSafeBase64String(signedHash);

        return urlString + 
            "?Policy=" + 
            urlSafePolicy + 
            "&Signature=" + 
            strSignedPolicy + 
            "&Key-Pair-Id=" + 
            PrivateKeyId;
    }
}
```

**Example Utility Methods for Signature Generation**  <a name="UtilityMethodsForSignatureGeneration"></a>
The following methods get the policy statement from a file and parse time intervals for signature generation\.  

```
public static string CreatePolicyStatement(string policyStmnt, 
   string resourceUrl, 
   DateTime startTime, 
   DateTime endTime, 
   string ipAddress)
   
{
   // Create the policy statement.
   FileStream streamPolicy = new FileStream(policyStmnt, FileMode.Open, FileAccess.Read);
   using (StreamReader reader = new StreamReader(streamPolicy))
   {
      string strPolicy = reader.ReadToEnd();

      TimeSpan startTimeSpanFromNow = (startTime - DateTime.Now);
      TimeSpan endTimeSpanFromNow = (endTime - DateTime.Now);
      TimeSpan intervalStart = 
         (DateTime.UtcNow.Add(startTimeSpanFromNow)) - 
         new DateTime(1970, 1, 1, 0, 0, 0, DateTimeKind.Utc);
      TimeSpan intervalEnd = 
         (DateTime.UtcNow.Add(endTimeSpanFromNow)) - 
         new DateTime(1970, 1, 1, 0, 0, 0, DateTimeKind.Utc);

      int startTimestamp = (int)intervalStart.TotalSeconds; // START_TIME
      int endTimestamp = (int)intervalEnd.TotalSeconds;  // END_TIME

      if (startTimestamp > endTimestamp)
         return "Error!";

      // Replace variables in the policy statement.
      strPolicy = strPolicy.Replace("RESOURCE", resourceUrl);
      strPolicy = strPolicy.Replace("START_TIME", startTimestamp.ToString());
      strPolicy = strPolicy.Replace("END_TIME", endTimestamp.ToString());
      strPolicy = strPolicy.Replace("IP_ADDRESS", ipAddress);
      strPolicy = strPolicy.Replace("EXPIRES", endTimestamp.ToString());
      return strPolicy;
   }   
}

public static TimeSpan GetDuration(string units, string numUnits)
{
   TimeSpan timeSpanInterval = new TimeSpan();
   switch (units)
   {
      case "seconds":
         timeSpanInterval = new TimeSpan(0, 0, 0, int.Parse(numUnits));
         break;
      case "minutes":
         timeSpanInterval = new TimeSpan(0, 0, int.Parse(numUnits), 0);
         break;
      case "hours":
         timeSpanInterval = new TimeSpan(0, int.Parse(numUnits), 0 ,0);
         break;
      case "days":
         timeSpanInterval = new TimeSpan(int.Parse(numUnits),0 ,0 ,0);
         break;
      default:
         Console.WriteLine("Invalid time units;" + 
            "use seconds, minutes, hours, or days");
         break;
   }
   return timeSpanInterval;
}

private static TimeSpan GetDurationByUnits(string durationUnits, 
   string startIntervalFromNow)
{
   switch (durationUnits)
   {
      case "seconds":
         return new TimeSpan(0, 0, int.Parse(startIntervalFromNow));
      case "minutes":
         return new TimeSpan(0, int.Parse(startIntervalFromNow), 0);
      case "hours":
         return new TimeSpan(int.Parse(startIntervalFromNow), 0, 0);
      case "days":
         return new TimeSpan(int.Parse(startIntervalFromNow), 0, 0, 0);
      default:
         return new TimeSpan(0, 0, 0, 0);
   }
}

public static string CopyExpirationTimeFromPolicy(string policyStatement)
{
   int startExpiration = policyStatement.IndexOf("EpochTime");
   string strExpirationRough = policyStatement.Substring(startExpiration + 
      "EpochTime".Length);
   char[] digits = { '0', '1', '2', '3', '4', '5', '6', '7', '8', '9' };
         
   List<char> listDigits = new List<char>(digits);
   StringBuilder buildExpiration = new StringBuilder(20);
         
   foreach (char c in strExpirationRough)
   {
      if (listDigits.Contains(c))
         buildExpiration.Append(c);
   }
   return buildExpiration.ToString();   
}
```

See also
+ [Create a URL Signature Using Perl](CreateURLPerl.md)
+ [Create a URL Signature Using PHP](CreateURL_PHP.md)
+ [Create a URL Signature Using Java](CFPrivateDistJavaDevelopment.md)