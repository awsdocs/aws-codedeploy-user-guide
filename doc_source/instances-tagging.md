# Tagging instances for deployment groups in CodeDeploy<a name="instances-tagging"></a>

To help manage your Amazon EC2 instances and on\-premises instances, you can use tags to assign your own metadata to each resource\. Tags enable you to categorize your instances in different ways \(for example, by purpose, owner, or environment\)\. This is useful when you have many instances\. You can quickly identify an instance or group of instances based on the tags you've assigned to them\. Each tag consists of a key and an optional value, both of which you define\. For more information, see [Tagging your Amazon EC2 resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)\.

To specify which instances are included in a CodeDeploy deployment group, you specify tags in one or more *tag groups*\. Instances that meet your tag criteria are the ones that the latest application revision is installed on when a deployment to that deployment group is created\.

**Note**  
You can also include Amazon EC2 Auto Scaling groups in deployment groups, but they are identified by their names rather than by tags applied to instances\. For information, see [Integrating CodeDeploy with Amazon EC2 Auto Scaling](integrations-aws-auto-scaling.md)\.

The criteria for instances in a deployment group can be as simple as a single tag in a single tag group\. It can be as complex as 10 tags each in a maximum of three tag groups\.

If you use a single tag group, any instance identified by at least one tag in the group is included in the deployment group\. If you use multiple tag groups, only instances that are identified by at least one tag in *each* of the tag groups are included\.

The following examples illustrate how tags and tag groups can be used to select the instances for a deployment group\.

**Topics**
+ [Example 1: Single tag group, single tag](#instances-tagging-example-1)
+ [Example 2: Single tag group, multiple tags](#instances-tagging-example-2)
+ [Example 3: Multiple tag groups, single tags](#instances-tagging-example-3)
+ [Example 4: Multiple tag groups, multiple tags](#instances-tagging-example-4)

## Example 1: Single tag group, single tag<a name="instances-tagging-example-1"></a>

You can specify a single tag in a single tag group: 


**Tag group 1**  

| Key | Value | 
| --- | --- | 
| Name | AppVersion\-ABC | 

Each instance that is tagged with `Name=AppVersion-ABC` is part of the deployment group, even if it has other tags applied\. 

CodeDeploy console setup view: 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/TaggingExample1-polaris.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

JSON structure:

```
"ec2TagSet": { 
         "ec2TagSetList": [ 
            [ 
               { 
                  "Type": "KEY_AND_VALUE",
                  "Key": "Name",
                  "Value": "AppVersion-ABC"
               }
            ]
         ]
      },
```

## Example 2: Single tag group, multiple tags<a name="instances-tagging-example-2"></a>

You can also specify multiple tags in a single tag group:


**Tag group 1**  

| Key | Value | 
| --- | --- | 
| Region | North | 
| Region | South | 
| Region | East | 

An instance that is tagged with any of these three tags is part of the deployment group, even if it has other tags applied\. If, for example, you had other instances tagged with `Region=West`, they would not be included in the deployment group\.

CodeDeploy console setup view: 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/TaggingExample2-polaris.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

JSON structure:

```
"ec2TagSet": { 
         "ec2TagSetList": [ 
            [ 
               { 
                  "Type": "KEY_AND_VALUE",
                  "Key": "Region",
                  "Value": "North"
               },
                              { 
                  "Type": "KEY_AND_VALUE",
                  "Key": "Region",
                  "Value": "South"
               },
                              { 
                  "Type": "KEY_AND_VALUE",
                  "Key": "Region",
                  "Value": "East"
               }
            ]
         ]
      },
```

## Example 3: Multiple tag groups, single tags<a name="instances-tagging-example-3"></a>

You can also use multiple sets of tag groups with a single key\-value pair in each to specify the criteria for instances in a deployment group\. When you use multiple tag groups in a deployment group, only instances that are identified by all the tag groups are included in the deployment group\. 


**Tag group 1**  

| Key | Value | 
| --- | --- | 
| Name | AppVersion\-ABC | 


**Tag group 2**  

| Key | Value | 
| --- | --- | 
| Region | North | 


**Tag group 3**  

| Key | Value | 
| --- | --- | 
| Type | t2\.medium | 

You might have instances in many regions and of various instance types tagged with `Name=AppVersion-ABC`\. In this example, only the instances also tagged with `Region=North` and `Type=t2.medium` are part of the deployment group\. 

CodeDeploy console setup view: 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/TaggingExample3-polaris.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

JSON structure:

```
"ec2TagSet": { 
         "ec2TagSetList": [ 
            [ 
               { 
                  "Type": "KEY_AND_VALUE",
                  "Key": "Name",
                  "Value": "AppVersion-ABC"
               }
            ],
            [ 
               { 
                  "Type": "KEY_AND_VALUE",
                  "Key": "Region",
                  "Value": "North"
               }
            ],
            [ 
               { 
                  "Type": "KEY_AND_VALUE",
                  "Key": "Type",
                  "Value": "t2.medium"
               }
            ],
         ]
      },
```

## Example 4: Multiple tag groups, multiple tags<a name="instances-tagging-example-4"></a>

When you use multiple tag groups with multiple tags in one or more group, an instance must match at least one of the tags in each of the groups\.


**Tag group 1**  

| Key | Value | 
| --- | --- | 
| Environment | Beta | 
| Environment | Staging | 


**Tag group 2**  

| Key | Value | 
| --- | --- | 
| Region | North | 
| Region | South | 
| Region | East | 


**Tag group 3**  

| Key | Value | 
| --- | --- | 
| Type | t2\.medium | 
| Type | t2\.large | 

In this example, to be included in the deployment group, an instance must be tagged with \(1\) `Environment=Beta` or `Environment=Staging`, with \(2\) `Region=North`, `Region=South`, or `Region=East`, and with \(3\) `Type=t2.medium` or `Type=t2.large`\.

To illustrate, instances with the following tag groups *would* be among those included in the deployment group:
+ `Environment=Beta`, `Region=North`,`Type=t2.medium`
+ `Environment=Staging`,`Region=East`,`Type=t2.large`
+ `Environment=Staging`,`Region=South`,`Type=t2.large`

Instances with the following tag groups would *not* be included in the deployment group\. The **highlighted** key values cause the instances to be excluded:
+ `Environment=Beta`, Region=**West**,`Type=t2.medium`
+ `Environment=Staging`,`Region=East`,Type=**t2\.micro**
+ Environment=**Production**,`Region=South`,`Type=t2.large`

CodeDeploy console setup view: 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/images/TaggingExample4-polaris.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codedeploy/latest/userguide/)

JSON structure:

```
"ec2TagSet": { 
         "ec2TagSetList": [ 
            [ 
               { 
                  "Type": "KEY_AND_VALUE",
                  "Key": "Environment",
                  "Value": "Beta"
               },
               { 
                  "Type": "KEY_AND_VALUE",
                  "Key": "Environment",
                  "Value": "Staging"
               }
            ],
            [ 
               { 
                  "Type": "KEY_AND_VALUE",
                  "Key": "Region",
                  "Value": "North"
               },
               { 
                  "Type": "KEY_AND_VALUE",
                  "Key": "Region",
                  "Value": "South"
               },
               { 
                  "Type": "KEY_AND_VALUE",
                  "Key": "Region",
                  "Value": "East"
               }
            ],
            [ 
               { 
                  "Type": "KEY_AND_VALUE",
                  "Key": "Type",
                  "Value": "t2.medium"
               },
               { 
                  "Type": "KEY_AND_VALUE",
                  "Key": "Type",
                  "Value": "t2.large"
               }
            ],
         ]
      },
```