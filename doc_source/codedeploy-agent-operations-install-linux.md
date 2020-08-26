# Install the CodeDeploy agent for Amazon Linux or RHEL<a name="codedeploy-agent-operations-install-linux"></a>

Sign in to the instance, and run the following commands, one at a time\. Running the first command, `sudo yum install`, first is considered best practice when using `yum` to install packages, but you can skip it if you do not wish to update all of your packages\.

**Note**  
In the fourth command, `/home/ec2-user` represents the default user name for an Amazon Linux or RHEL Amazon EC2 instance\. If your instance was created using a custom AMI, the AMI owner might have specified a different default user name\. 

```
sudo yum update
```

```
sudo yum install ruby
```

```
sudo yum install wget
```

```
cd /home/ec2-user
```

```
wget https://bucket-name.s3.region-identifier.amazonaws.com/latest/install
```

*bucket\-name* is the name of the Amazon S3 bucket that contains the CodeDeploy Resource Kit files for your region\. *region\-identifier* is the identifier for your region\. For example, for the US East \(Ohio\) Region, replace *bucket\-name* with `aws-codedeploy-us-east-2` and replace *region\-identifier* with `us-east-2`\. For a list of bucket names and region identifiers, see [Resource kit bucket names by Region](resource-kit.md#resource-kit-bucket-names)\.

```
chmod +x ./install
```

To install the latest version of the CodeDeploy agent:
+ 

  ```
  sudo ./install auto
  ```

To install a specific version of the CodeDeploy agent:
+ 

  ```
  sudo ./install auto -v releases/codedeploy-agent-###.rpm
  ```

To check that the service is running, run the following command:

```
sudo service codedeploy-agent status
```

If the CodeDeploy agent is installed and running, you should see a message like `The AWS CodeDeploy agent is running`\.

If you see a message like `error: No AWS CodeDeploy agent running`, start the service and run the following two commands, one at a time:

```
sudo service codedeploy-agent start
```

```
sudo service codedeploy-agent status
```