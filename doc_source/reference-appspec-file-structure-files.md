# AppSpec 'files' section \(EC2/On\-Premises deployments only\)<a name="reference-appspec-file-structure-files"></a>

Provides information to CodeDeploy about which files from your application revision should be installed on the instance during the deployment's **Install** event\. This section is required only if you are copying files from your revision to locations on the instance during deployment\. 

This section has the following structure:

```
files:
  - source: source-file-location-1
    destination: destination-file-location-1
file_exists_behavior: DISALLOW|OVERWRITE|RETAIN
```

Multiple `source` and `destination` pairs can be set\.

The `source` instruction identifies a file or directory from your revision to copy to the instance:
+ If `source` refers to a file, only the specified files are copied to the instance\.
+ If `source` refers to a directory, then all files in the directory are copied to the instance\.
+ If `source` is a single slash \("/" for Amazon Linux, RHEL, and Ubuntu Server instances, or "\\" for Windows Server instances\), then all of the files from your revision are copied to the instance\.

The paths used in `source` are relative to the `appspec.yml` file, which should be at the root of your revision\. For details on the file structure of a revision, see [Plan a revision for CodeDeploy](application-revisions-plan.md)\.

The `destination` instruction identifies the location on the instance where the files should be copied\. This must be a fully qualified path such as `/root/destination/directory` \(on Linux, RHEL, and Ubuntu\) or `c:\destination\folder` \(on Windows\)\.

`source` and `destination` are each specified with a string\.

The `file_exists_behavior` instruction is optional, and specifies how CodeDeploy handles files that already exist in a deployment target location but weren't part of the previous successful deployment\. This setting can take any of the following values:
+ DISALLOW: The deployment fails\. This is also the default behavior if no option is specified\. 
+ OVERWRITE: The version of the file from the application revision currently being deployed replaces the version already on the instance\. 
+ RETAIN: The version of the file already on the instance is kept and used as part of the new deployment\.

When using the `file_exists_behavior` setting, understand that this setting:
+ can only be specified once, and applies to all files and directories listed under `files:`\.
+ takes precedence over the `--file-exists-behavior` AWS CLI option and the `fileExistsBehavior` API option \(both of which are also optional\)\.

Here's an example `files` section for an Amazon Linux, Ubuntu Server, or RHEL instance\.

```
files:
  - source: Config/config.txt
    destination: /webapps/Config
  - source: source
    destination: /webapps/myApp
```

In this example, the following two operations are performed during the **Install** event:

1. Copy the `Config/config.txt` file in your revision to the `/webapps/Config/config.txt` path on the instance\.

1. Recursively copy all of the files in your revision's `source` directory to the `/webapps/myApp` directory on the instance\.

## 'Files' section examples<a name="reference-appspec-file-structure-files-examples"></a>

The following examples show how to specify the `files` section\. Although these examples describe Windows Server file and directory \(folder\) structures, they can easily be adapted for Amazon Linux, Ubuntu Server, and RHEL instances\.

**Note**  
Only EC2/On\-Premises deployments use the `files` section\. It does not apply to AWS Lambda deployments\.

For the following examples, we assume these files appear in the bundle in the root of `source`:
+ `appspec.yml`
+ `my-file.txt`
+ `my-file-2.txt`
+ `my-file-3.txt`

```
# 1) Copy only my-file.txt to the destination folder c:\temp.
#
files:
  - source: .\my-file.txt
    destination: c:\temp
#
# Result:
#   c:\temp\my-file.txt
#
# ---------------------
#
# 2) Copy only my-file-2.txt and my-file-3.txt to the destination folder c:\temp.
#
files:
  - source: my-file-2.txt
    destination: c:\temp
  - source: my-file-3.txt
    destination: c:\temp
#
# Result:
#   c:\temp\my-file-2.txt
#   c:\temp\my-file-3.txt
#
# ---------------------
#
# 3) Copy my-file.txt, my-file-2.txt, and my-file-3.txt (along with the appspec.yml file) to the destination folder c:\temp.
#
files:
  - source: \
    destination: c:\temp
#
# Result:
#   c:\temp\appspec.yml
#   c:\temp\my-file.txt
#   c:\temp\my-file-2.txt
#   c:\temp\my-file-3.txt
```

For the following examples, we assume the `appspec.yml` appears in the bundle in the root of `source` along with a folder named `my-folder` that contains three files:
+ `appspec.yml`
+ `my-folder\my-file.txt`
+ `my-folder\my-file-2.txt`
+ `my-folder\my-file-3.txt`

```
# 4) Copy the 3 files in my-folder (but do not copy my-folder itself) to the destination folder c:\temp. 
#
files:
  - source: .\my-folder
    destination: c:\temp
#
# Result:
#   c:\temp\my-file.txt
#   c:\temp\my-file-2.txt
#   c:\temp\my-file-3.txt
#
# ---------------------
#
# 5) Copy my-folder and its 3 files to my-folder within the destination folder c:\temp.
#
files:
  - source: .\my-folder
    destination: c:\temp\my-folder
#
# Result:
#   c:\temp\my-folder\my-file.txt
#   c:\temp\my-folder\my-file-2.txt
#   c:\temp\my-folder\my-file-3.txt
#
# ---------------------
#
# 6) Copy the 3 files in my-folder to other-folder within the destination folder c:\temp.
#
files:
  - source: .\my-folder
    destination: c:\temp\other-folder
#
# Result:
#   c:\temp\other-folder\my-file.txt
#   c:\temp\other-folder\my-file-2.txt
#   c:\temp\other-folder\my-file-3.txt	
#
# ---------------------
#
# 7) Copy only my-file-2.txt and my-file-3.txt to my-folder within the destination folder c:\temp.
#
files:
  - source: .\my-folder\my-file-2.txt
    destination: c:\temp\my-folder
  - source: .\my-folder\my-file-3.txt
    destination: c:\temp\my-folder
#
# Result:
#   c:\temp\my-folder\my-file-2.txt
#   c:\temp\my-folder\my-file-3.txt
#
# ---------------------
#
# 8) Copy only my-file-2.txt and my-file-3.txt to other-folder within the destination folder c:\temp.
#
files:
  - source: .\my-folder\my-file-2.txt
    destination: c:\temp\other-folder
  - source: .\my-folder\my-file-3.txt
    destination: c:\temp\other-folder
#
# Result:
#   c:\temp\other-folder\my-file-2.txt
#   c:\temp\other-folder\my-file-3.txt
#
# ---------------------
#
# 9) Copy my-folder and its 3 files (along with the appspec.yml file) to the destination folder c:\temp. If any of the files already exist on the instance, overwrite them.
#
files:
  - source: \
    destination: c:\temp
file_exists_behavior: OVERWRITE
#
# Result:
#   c:\temp\appspec.yml
#   c:\temp\my-folder\my-file.txt
#   c:\temp\my-folder\my-file-2.txt
#   c:\temp\my-folder\my-file-3.txt
```