# AppSpec 'permissions' section \(EC2/On\-Premises deployments only\)<a name="reference-appspec-file-structure-permissions"></a>

The `'permissions'` section specifies how special permissions, if any, should be applied to the files and directories/folders in the `'files'` section after they are copied to the instance\. You can specify multiple `object` instructions\. This section is optional\. It applies to Amazon Linux, Ubuntu Server, and RHEL instances only\.

**Note**  
The `'permissions'` section is used for EC2/On\-Premises deployments only\. It is not used for AWS Lambda or Amazon ECS deployments\.

This section has the following structure:

```
permissions:
  - object: object-specification
    pattern: pattern-specification
    except: exception-specification
    owner: owner-account-name
    group: group-name
    mode: mode-specification
    acls: 
      - acls-specification 
    context:
      user: user-specification
      type: type-specification
      range: range-specification
    type:
      - object-type
```

The instructions are as follows:
+ `object` – Required\. This is a set of file system objects \(files or directories/folders\) that the specified permissions are applied to after the file system objects are copied to the instance\.

  Specify `object` with a string\.
+ `pattern` – Optional\. Specifies a pattern to apply permissions\. If not specified or specified with the special characters **"\*\*"**, the permissions are applied to all matching files or directories, depending on the `type`\. 

  Specify `pattern` with a string with quotation marks \(""\)\.
+ `except` – Optional\. Specifies any files or directories that are exceptions to `pattern`\. 

  Specify `except` with a comma\-separated list of strings inside square brackets\.
+ `owner` – Optional\. The name of the owner of `object`\. If not specified, all existing owners applied to the original file or directory/folder structure remain unchanged after the copy operation\.

  Specify `owner` with a string\.
+ `group` – Optional\. The name of the group for `object`\. If not specified, all existing groups applied to the original file or directory/folder structure remain unchanged after the copy operation\.

  Specify `group` with a string\.
+ `mode` – Optional\. An integer specifying the octal mode for the permissions to be applied to `object`\. For example, **644** represents read and write permissions for the owner, read\-only permissions for the group, and read\-only permissions for all other users\. **4755** represents the setuid attribute is set, full control permissions for the owner, read and execute permissions for the group, and read and execute permissions for all other users\. \(For more examples, see the Linux `chmod` command documentation\.\) If `mode` is not specified, all existing modes applied to the original file or directory/folder structure remain unchanged after the copy operation\.

  Specify `mode` with a string\.
+ `acls` – Optional\. A list of character strings representing one or more access control list \(ACL\) entries applied to `object`\. For example, **u:bob:rw** represents read and write permissions for user **bob**\. \(For more examples, see ACL entry format examples in the Linux `setfacl` command documentation\.\) You can specify multiple ACL entries\. If `acls` is not specified, any existing ACLs applied to the original file or directory/folder structure remain unchanged after the copy operation\. These replace any existing ACLs\.

  Specify an `acls` with a dash \(\-\), followed by a space, and then a string \(for example, `- u:jane:rw`\)\. If you have more than one ACL, each is specified on a separate line\.
**Note**  
Setting unnamed users, unnamed groups, or other similar ACL entries causes the AppSpec file to fail\. Use `mode` to specify these types of permissions instead\.
+ `context` – Optional\. For Security\-Enhanced Linux \(SELinux\)\-enabled instances, a list of security\-relevant context labels to apply to the copied objects\. Labels are specified as keys containing `user`, `type`, and `range`\. \(For more information, see the SELinux documentation\.\) Each key is entered with a string\. If not specified, any existing labels applied to the original file or directory/folder structure remain unchanged after the copy operation\.
  + `user` – Optional\. The SELinux user\.
  + `type` – Optional\. The SELinux type name\.
  + `range` – Optional\. The SELinux range specifier\. This has no effect unless Multi\-Level Security \(MLS\) and Multi\-Category Security \(MCS\) are enabled on the machine\. If not enabled, `range` defaults to **s0**\.

  Specify `context` with a string \(for example, `user: unconfined_u`\)\. Each `context` is specified on a seperate line\.
+ `type` – Optional\. The types of objects to which to apply the specified permissions\. `type` is a string that can be set to **file** or **directory**\. If **file** is specified, the permissions are applied only to files that are immediately contained in `object` after the copy operation \(and not to `object` itself\)\. If **directory** is specified, the permissions are recursively applied to all directories/folders that are anywhere in `object` after the copy operation \(but not to `object` itself\)\.

  Specify `type` with a dash \(\-\), followed by a space, and then a string \(for example, `- file`\)\.

## 'Permissions' section example<a name="reference-appspec-file-structure-permissions-example"></a>

The following example shows how to specify the `'permissions'` section with the `object`, `pattern`, `except`, `owner`, `mode`, and `type` instructions\. This example applies to Amazon Linux, Ubuntu Server, and RHEL instances only\. In this example, assume the following files and folders are copied to the instance in this hierarchy:

```
/tmp
  `-- my-app
       |-- my-file-1.txt
       |-- my-file-2.txt
       |-- my-file-3.txt
       |-- my-folder-1
       |     |-- my-file-4.txt
       |     |-- my-file-5.txt
       |     `-- my-file-6.txt
       `-- my-folder-2
             |-- my-file-7.txt
             |-- my-file-8.txt
             |-- my-file-9.txt
	           `-- my-folder-3
```

The following AppSpec file shows how to set permissions on these files and folders after they are copied:

```
version: 0.0
os: linux
# Copy over all of the folders and files with the permissions they
#  were originally assigned.
files:
  - source: ./my-file-1.txt
    destination: /tmp/my-app
  - source: ./my-file-2.txt
    destination: /tmp/my-app
  - source: ./my-file-3.txt
    destination: /tmp/my-app
  - source: ./my-folder-1
    destination: /tmp/my-app/my-folder-1
  - source: ./my-folder-2
    destination: /tmp/my-app/my-folder-2
# 1) For all of the files in the /tmp/my-app folder ending in -3.txt
#  (for example, just my-file-3.txt), owner = adm, group = wheel, and
#  mode = 464 (-r--rw-r--).
permissions:
  - object: /tmp/my-app
    pattern: "*-3.txt"
    owner: adm
    group: wheel
    mode: 464
    type:
      - file
# 2) For all of the files ending in .txt in the /tmp/my-app
#  folder, but not for the file my-file-3.txt (for example,
#  just my-file-1.txt and my-file-2.txt),
#  owner = ec2-user and mode = 444 (-r--r--r--).
  - object: /tmp/my-app
    pattern: "*.txt"
    except: [my-file-3.txt]
    owner: ec2-user
    mode: 444
    type:
      - file
# 3) For all the files in the /tmp/my-app/my-folder-1 folder except
#  for my-file-4.txt and my-file-5.txt, (for example,
#  just my-file-6.txt), owner = operator and mode = 646 (-rw-r--rw-).
  - object: /tmp/my-app/my-folder-1
    pattern: "**"
    except: [my-file-4.txt, my-file-5.txt]
    owner: operator
    mode: 646
    type:
      - file
# 4) For all of the files that are immediately under
#  the /tmp/my-app/my-folder-2 folder except for my-file-8.txt,
#  (for example, just my-file-7.txt and
#  my-file-9.txt), owner = ec2-user and mode = 777 (-rwxrwxrwx).
  - object: /tmp/my-app/my-folder-2
    pattern: "**"
    except: [my-file-8.txt]
    owner: ec2-user
    mode: 777
    type:
      - file
# 5) For all folders at any level under /tmp/my-app that contain
#  the name my-folder but not
#  /tmp/my-app/my-folder-2/my-folder-3 (for example, just
#  /tmp/my-app/my-folder-1 and /tmp/my-app/my-folder-2),
#  owner = ec2-user and mode = 555 (dr-xr-xr-x).
  - object: /tmp/my-app
    pattern: "*my-folder*"
    except: [tmp/my-app/my-folder-2/my-folder-3]
    owner: ec2-user
    mode: 555
    type:
      - directory
# 6) For the folder /tmp/my-app/my-folder-2/my-folder-3,
#  group = wheel and mode = 564 (dr-xrw-r--).
  - object: /tmp/my-app/my-folder-2/my-folder-3
    group: wheel
    mode: 564
    type:
      - directory
```

The resulting permissions are as follows:

```
-r--r--r-- ec2-user root  my-file-1.txt
-r--r--r-- ec2-user root  my-file-2.txt
-r--rw-r-- adm      wheel my-file-3.txt

dr-xr-xr-x ec2-user root  my-folder-1
-rw-r--r-- root     root  my-file-4.txt
-rw-r--r-- root     root  my-file-5.txt
-rw-r--rw- operator root  my-file-6.txt

dr-xr-xr-x ec2-user root  my-folder-2
-rwxrwxrwx ec2-user root  my-file-7.txt
-rw-r--r-- root     root  my-file-8.txt
-rwxrwxrwx ec2-user root  my-file-9.txt

dr-xrw-r-- root     wheel my-folder-3
```

The following example shows how to specify the `'permissions'` section with the addition of the `acls` and `context` instructions\. This example applies to Amazon Linux, Ubuntu Server, and RHEL instances only\.

```
permissions:
  - object: /var/www/html/WordPress
    pattern: "**"
    except: [/var/www/html/WordPress/ReadMe.txt]
    owner: bob
    group: writers
    mode: 644
    acls: 
      - u:mary:rw
      - u:sam:rw
      - m::rw
    context:
      user: unconfined_u
      type: httpd_sys_content_t
      range: s0
    type:
      - file
```