# AppSpec 'files' section \(EC2/On\-Premises deployments only\)<a name="reference-appspec-file-structure-files"></a>

Provides information to CodeDeploy about which files from your application revision should be installed on the instance during the deployment's **Install** event\. This section is required only if you are copying files from your revision to locations on the instance during deployment\. 

This section has the following structure:

```
files:
  - source: source-file-location
    destination: destination-file-location
```

Multiple `source` and `destination` pairs can be set\.

The `source` instruction identifies a file or directory from your revision to copy to the instance:
+ If `source` refers to a file, only the specified files are copied to the instance\.
+ If `source` refers to a directory, then all files in the directory are copied to the instance\.
+ If `source` is a single slash \("/" for Amazon Linux, RHEL, and Ubuntu Server instances, or "\\" for Windows Server instances\), then all of the files from your revision are copied to the instance\.

The paths used in `source` are relative paths, starting from the root of your revision\.

The `destination` instruction identifies the location on the instance where the files should be copied\. This must be a fully qualified path\.

`source` and `destination` are each specified with a string\.

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
# 9) Copy my-folder and its 3 files (along with the appspec.yml file) to the destination folder c:\temp.
#
files:
  - source: \
    destination: c:\temp
#
# Result:
#   c:\temp\appspec.yml
#   c:\temp\my-folder\my-file.txt
#   c:\temp\my-folder\my-file-2.txt
#   c:\temp\my-folder\my-file-3.txt
```