# Uninstall the CodeDeploy agent<a name="codedeploy-agent-operations-uninstall"></a>

You can remove the CodeDeploy agent from instances when it is no longer needed or when you want to perform a fresh installation\.

## Uninstall the CodeDeploy agent from Amazon Linux or RHEL<a name="codedeploy-agent-operations-uninstall-linux"></a>

To uninstall the CodeDeploy agent, sign in to the instance and run the following command:

```
sudo yum erase codedeploy-agent
```

## Uninstall the CodeDeploy agent from Ubuntu Server<a name="codedeploy-agent-operations-uninstall-ubuntu"></a>

To uninstall the CodeDeploy agent, sign in to the instance and run the following command:

```
sudo dpkg --purge codedeploy-agent
```

## Uninstall the CodeDeploy agent from Windows Server<a name="codedeploy-agent-operations-uninstall-windows"></a>

To uninstall the CodeDeploy agent, sign in to the instance and run the following three commands, one at a time:

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