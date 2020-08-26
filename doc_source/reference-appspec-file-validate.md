# Validate your AppSpec File and file location<a name="reference-appspec-file-validate"></a>

 **File syntax** 

You can use the AWS\-provided AppSpec Assistant script to validate the contents of an AppSpec file\. You can find the script along with AppSpec file templates on [GitHub](https://github.com/aws-samples/aws-codedeploy-appspec-assistant)\.

You can also use a browser\-based tool such as [YAML lint](http://www.yamllint.com/) or [Online YAML parser](http://yaml-online-parser.appspot.com/) to help you check your YAML syntax\.

 **File location** 

To verify that you have placed your AppSpec file in the root directory of the application's source content's directory structure, run one of the following commands:

On local Linux, macOS, or Unix instances:

```
ls path/to/root/directory/appspec.yml
```

If the AppSpec file is not located there, a "No such file or directory" error is displayed\.

On local Windows instances:

```
dir path\to\root\directory\appspec.yml
```

If the AppSpec file is not located there, a "File Not Found" error is displayed\.