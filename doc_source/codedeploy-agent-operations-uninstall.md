--------

 The procedures in this guide support the new console design\. If you choose to use the older version of the console, you will find many of the concepts and basic procedures in this guide still apply\. To access help in the new console, choose the information icon\. 

--------

# Uninstall the AWS CodeDeploy Agent<a name="codedeploy-agent-operations-uninstall"></a>

You can remove the AWS CodeDeploy agent from instances when it is no longer needed or when you want to perform a fresh installation\.

## Uninstall the AWS CodeDeploy Agent from Amazon Linux or RHEL<a name="codedeploy-agent-operations-uninstall-linux"></a>

To uninstall the AWS CodeDeploy agent, sign in to the instance and run the following command:

```
sudo yum erase codedeploy-agent
```

## Uninstall the AWS CodeDeploy Agent from Ubuntu Server<a name="codedeploy-agent-operations-uninstall-ubuntu"></a>

To uninstall the AWS CodeDeploy agent, sign in to the instance and run the following command:

```
sudo dpkg --purge codedeploy-agent
```

## Uninstall the AWS CodeDeploy Agent from Windows Server<a name="codedeploy-agent-operations-uninstall-windows"></a>

To uninstall the AWS CodeDeploy agent, sign in to the instance and run the following three commands, one at a time:

```
wmic
```

```
product where name="CodeDeploy Host Agent" call uninstall /nointeractive
```

```
exit
```

You can also sign in to the instance, and in **Control Panel**, open **Programs and Features**, choose **CodeDeploy Host Agent**, and then choose **Uninstall**\.