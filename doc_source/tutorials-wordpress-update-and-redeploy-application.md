# Step 5: Update and Redeploy Your WordPress Application<a name="tutorials-wordpress-update-and-redeploy-application"></a>

Now that you've successfully deployed your application revision, update the WordPress code on the development machine, and then use AWS CodeDeploy to redeploy the site\. Afterward, you should see the code changes on the Amazon EC2 instance\.

**Topics**
+ [Set Up the WordPress Site](#tutorials-wordpress-update-and-redeploy-application-configure-and-install)
+ [Modify the Site](#tutorials-wordpress-update-and-redeploy-application-modify-code)
+ [Redeploy the Site](#tutorials-wordpress-update-and-redeploy-application-deploy-updates)

## Set Up the WordPress Site<a name="tutorials-wordpress-update-and-redeploy-application-configure-and-install"></a>

To see the effects of the code change, finish setting up the WordPress site so that you have a fully functional installation\.

1. Type your site's URL into your web browser\. The URL is the public DNS address of the Amazon EC2 instance plus a `/WordPress` extension\. For this example WordPress site \(and example Amazon EC2 instance public DNS address\), the URL is **http://ec2\-01\-234\-567\-890\.compute\-1\.amazonaws\.com/WordPress**\.

1. If you haven't set up the site yet, the WordPress default welcome page appears\. Choose **Let's go\!**\.

1. To use the default MySQL database, on the database configuration page, type the following values:
   + **Database Name**: **test**
   + **User Name**: **root**
   + **Password**: Leave blank\.
   + **Database Host**: **localhost**
   + **Table Prefix**: **wp\_**

   Choose **Submit** to set up the database\.

1. Continue the site setup\. On the **Welcome** page, fill in any values you want, and choose **Install WordPress**\. When the installation is complete, you can sign in to your dashboard\.

**Important**  
 During the deployment of the WordPress application, the **change\_permissions\.sh** script updated permissions of the `/tmp/WordPress` folder so anyone can write to it\. Now is a good time to run the following command to restrict permissions so that only you, the owner, can write to it:  

```
chmod -R 755 /var/www/html/WordPress
```

## Modify the Site<a name="tutorials-wordpress-update-and-redeploy-application-modify-code"></a>

To modify the WordPress site, go to the application's folder on your development machine:

```
cd /tmp/WordPress
```

To modify some of the site's colors, in the `wp-content/themes/twentyfifteen/style.css` file, use a text editor or sed to change `#fff` to `#768331`\. 

On Linux or other systems with GNU sed, use:

```
sed -i 's/#fff/#768331/g' wp-content/themes/twentyfifteen/style.css
```

On macOS, Unix, or other systems with BSD sed, use:

```
sed -i '' 's/#fff/#768331/g' wp-content/themes/twentyfifteen/style.css
```

## Redeploy the Site<a name="tutorials-wordpress-update-and-redeploy-application-deploy-updates"></a>

Now that you've modified the site's code, use Amazon S3 and AWS CodeDeploy to redeploy the site\.

Bundle and upload the changes to Amazon S3, as described in [Bundle the Application's Files into a Single Archive File and Push the Archive File](tutorials-wordpress-upload-application.md#tutorials-wordpress-upload-application-bundle-and-push-archive)\. \(As you follow those instructions, remember that you do not need to create an application\.\) Give the new revision the same key as before \(**WordPressApp\.zip**\)\. Upload it to the same Amazon S3 bucket you created earlier \(for example, **codedeploydemobucket**\)\.

Use the AWS CLI, the AWS CodeDeploy console, or the AWS CodeDeploy APIs to redeploy the site\.

**Topics**
+ [To redeploy the site \(CLI\)](#tutorials-wordpress-update-and-redeploy-application-deploy-updates-cli)
+ [To redeploy the site \(console\)](#tutorials-wordpress-update-and-redeploy-application-deploy-updates-console)

### To redeploy the site \(CLI\)<a name="tutorials-wordpress-update-and-redeploy-application-deploy-updates-cli"></a>

Call the create\-deployment command to create a deployment based on the newly uploaded revision\. Use the application named **WordPress\_App**, the deployment configuration named **CodeDeployDefault\.OneAtATime**, the deployment group named **WordPress\_DepGroup**, and the revision named **WordPressApp\.zip** in the bucket named **codedeploydemobucket**:

```
 aws deploy create-deployment \
  --application-name WordPress_App \
  --deployment-config-name CodeDeployDefault.OneAtATime \
  --deployment-group-name WordPress_DepGroup \  
  --s3-location bucket=codedeploydemobucket,bundleType=zip,key=WordPressApp.zip
```

You can check the status of the deployment, as described in [Monitor and Troubleshoot Your Deployment](tutorials-wordpress-deploy-application.md#tutorials-wordpress-deploy-application-monitor)\.

After AWS CodeDeploy has redeployed the site, revisit the site in your web browser to verify the colors have been changed\. \(You might need to refresh your browser\.\) If the colors have been changed, congratulations\! You have successfully modified and redeployed your site\!

### To redeploy the site \(console\)<a name="tutorials-wordpress-update-and-redeploy-application-deploy-updates-console"></a>

1. Sign in to the AWS Management Console and open the AWS CodeDeploy console at [https://console\.aws\.amazon\.com/codedeploy](https://console.aws.amazon.com/codedeploy)\.
**Note**  
Sign in with the same account or IAM user information you used in [Getting Started with AWS CodeDeploy](getting-started-codedeploy.md)\.

1. On the AWS CodeDeploy menu, choose **Deployments**\.

1. Choose **Create deployment**\. 

1. On the **Create deployment** page:

   1. In the **Application** list, choose **WordPress\_App**\.
**Note**  
If no entries are displayed, make sure the correct region is selected\. On the navigation bar, in the region selector, choose one of the regions listed in [Region and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codedeploy_region) in the *AWS General Reference*\. AWS CodeDeploy is supported in these regions only\.

   1. In the **Deployment group** list, choose **WordPress\_DepGroup**\.

   1. In the **Repository type** area, choose **My application is stored in Amazon S3**, and then copy your revision's Amazon S3 link into the **Revision location** box\. To find the link value: 

      1. In a separate browser tab:

         Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

          Browse to and open **codedeploydemobucket**, and then choose your revision, **WordPressApp\.zip**\. 

      1.  If the **Properties** pane is not visible in the Amazon S3 console, choose the **Properties** button\. 

      1.  In the **Properties** pane, copy the value of the **Link** field into the **Revision location** box in the AWS CodeDeploy console\. 

   1. If a message appears saying the file type could not be detected, choose **\.zip**\. 

   1. Leave the **Deployment description** box blank\.

   1. In the **Deployment configuration** list, choose **CodeDeployDefault\.OneAtATime**, and then choose **Deploy**\. 

      To update the deployment's status, choose the **Refresh** button above the table\.

      You can check the status of the deployment, as described in [Monitor and Troubleshoot Your Deployment](tutorials-wordpress-deploy-application.md#tutorials-wordpress-deploy-application-monitor)\.

      After AWS CodeDeploy has redeployed the site, revisit the site in your web browser to verify the colors have been changed\. \(You might need to refresh your browser\.\) If the colors have been changed, congratulations\! You have successfully modified and redeployed your site\!