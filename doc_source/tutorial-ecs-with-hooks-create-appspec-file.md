# Step 4: Update your AppSpec file<a name="tutorial-ecs-with-hooks-create-appspec-file"></a>

 In this section, you update your AppSpec file with a `Hooks` section\. In the `Hooks` section, you specify a Lambda function for the `AfterAllowTestTraffic` lifecycle hook\. 

**To update your AppSpec file**

1.  Open the AppSpec file file you created in [ Step 2: Create the AppSpec file ](tutorial-ecs-create-appspec-file.md) of the [Tutorial: Deploy an Amazon ECS service](tutorial-ecs-deployment.md)\. 

1.  Update the `TaskDefinition` property with the task definition ARN you noted in [ Step 2: Update your Amazon ECS application](tutorial-ecs-with-hooks-update-the-ecs-application.md)\. 

1. Copy and paste the `Hooks` section into your AppSpec file file\. Update the ARN after `AfterAllowTestTraffic` with the ARN of the Lambda function that you noted in [Step 3: Create a lifecycle hook Lambda function](tutorial-ecs-with-hooks-create-hooks.md)\. 

------
#### [ JSON AppSpec ]

   ```
   {
     "version": 0.0,
     "Resources": [
       {
         "TargetService": {
           "Type": "AWS::ECS::Service",
           "Properties": {
             "TaskDefinition": "arn:aws:ecs:aws-region-id:aws-account-id::task-definition/ecs-demo-task-definition:revision-number",
             "LoadBalancerInfo": {
               "ContainerName": "sample-website",
               "ContainerPort": 80
             }
           }
         }
       }
     ],
     "Hooks": [
       {
         "AfterAllowTestTraffic": "arn:aws:lambda:aws-region-id:aws-account-id:function:AfterAllowTestTraffic"
       }
     ]
   }
   ```

------
#### [ YAML AppSpec ]

   ```
   version: 0.0
   Resources:
     - TargetService:
         Type: AWS::ECS::Service
         Properties:
           TaskDefinition: "arn:aws:ecs:aws-region-id:aws-account-id::task-definition/ecs-demo-task-definition:revision-number"
           LoadBalancerInfo:
             ContainerName: "sample-website"
             ContainerPort: 80
   Hooks:
     - AfterAllowTestTraffic: "arn:aws:lambda:aws-region-id:aws-account-id:function:AfterAllowTestTraffic"
   ```

------

1.  Save your AppSpec file and upload to its S3 bucket\. 