# Accessing the Request Body by Choosing the Include Body Option<a name="lambda-include-body-access"></a>

You can opt to have Lambda@Edge expose the body in a request for writable HTTP methods \(POST, PUT, DELETE, and so on\), so that you can access it in your Lambda function\. You can choose read\-only access, or you can specify that you’ll replace the body\.

To enable this option, choose **Include Body** when you create a CloudFront trigger for your function that's for a viewer request or origin request event\. For more information, see [Adding Triggers for a Lambda@Edge Function](lambda-edge-add-triggers.md), or to learn about using **Include Body** with your function, see [Lambda@Edge Event Structure](lambda-event-structure.md)\.

Scenarios when you might want to use this feature include the following:
+ Processing web forms, like "contact us" forms, without sending customer input data back to origin servers\.
+ Gathering web beacon data that's sent by viewer browsers and processing it at the edge\.

For sample code, see [Lambda@Edge Example Functions](lambda-examples.md)\.

**Note**  
If the request body is large, Lambda@Edge truncates it\. For detailed information about the maximum size and truncation, see [Maximum Size for Body with the Include Body Option](lambda-requirements-limits.md#lambda-requirements-size-body-access)\.