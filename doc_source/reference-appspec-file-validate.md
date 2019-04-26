# Validate Your AppSpec File and File Location<a name="reference-appspec-file-validate"></a>

 **File syntax** 

CodeDeploy does not provide a tool for validating the contents of an AppSpec File\. Instead, consider using a browser\-based tool such as [YAML Lint](http://www.yamllint.com/) or [Online YAML Parser](http://yaml-online-parser.appspot.com/) to help you check your YAML syntax\.

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