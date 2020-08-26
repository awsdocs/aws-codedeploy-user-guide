# Step 5: Verify your deployment<a name="tutorials-on-premises-instance-5-verify-deployment"></a>

To verify the deployment was successful, follow the instructions in [View CodeDeploydeployment details ](deployments-view-details.md), and then return to this page\.

If the deployment was successful, you'll find four text files in the `c:\temp\CodeDeployExample` folder \(for Windows Server\) or `/tmp/CodeDeployExample` \(for Ubuntu Server and RHEL\)\. 

If the deployment failed, follow the troubleshooting steps in [View instance details with CodeDeploy](instances-view-details.md) and [Troubleshoot instance issues](troubleshooting-ec2-instances.md)\. Make any required fixes, rebundle and upload your application revision, and then try the deployment again\.