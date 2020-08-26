# Specify information about a revision stored in an Amazon S3 bucket<a name="deployments-create-console-s3"></a>

If you are following the steps in [Create an EC2/On\-Premises Compute Platform deployment \(console\)](deployments-create-console.md), follow these steps to add details about an application revision stored in an Amazon S3 bucket\.

1. Copy your revision's Amazon S3 link into **Revision location**\. To find the link value:

   1. In a separate browser tab:

      Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

      Browse to and choose your revision\.

   1. If the **Properties** pane is not visible, choose the **Properties** button\.

   1. In the **Properties** pane, copy the value of the **Link** field into the **Revision location** box in the CodeDeploy console\.

   To specify an ETag \(a file checksum\) as part of the revision location:
   + If the **Link** field value ends in **?versionId=*versionId***, add **&etag=** and the ETag to the end of the **Link** field value\.
   + If the **Link** field value does not specify a version ID, add **?etag=** and the ETag to the end of the **Link** field value\.
**Note**  
Although it's not as easy as copying the value of the **Link** field, you can also type the revision location in one of the following formats:  
**s3://*bucket\-name*/*folders*/*objectName***  
**s3://*bucket\-name*/*folders*/*objectName*?versionId=*versionId***  
**s3://*bucket\-name*/*folders*/*objectName*?etag=*etag***  
**s3://*bucket\-name*/*folders*/*objectName*?versionId=*versionId*&etag=*etag***  
***bucket\-name*\.s3\.amazonaws\.com/*folders*/*objectName***

1. If a message appears in the **File type** list that says the file type could not be detected, choose the revision's file type\. Otherwise, accept the detected file type\.