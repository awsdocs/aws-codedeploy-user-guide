# Step 2: Create the AppSpec file<a name="tutorial-ecs-create-appspec-file"></a>

 In this section, you create your AppSpec file and upload it to the Amazon S3 bucket you created in the [Prerequisites](tutorial-ecs-prereqs.md) section\. The AppSpec file for an Amazon ECS deployment specifies your task definition, container name, and container port\. For more information, see [ AppSpec File example for an Amazon ECS deployment ](reference-appspec-file-example.md#appspec-file-example-ecs) and [ AppSpec 'resources' section for Amazon ECS deployments ](reference-appspec-file-structure-resources.md#reference-appspec-file-structure-resources-ecs)\. 

**To create your AppSpec file**

1.  If you want to create your AppSpec file using YAML, create a file named `appspec.yml`\. If you want to create your AppSpec file using JSON, create a file named `appspec.json`\. 

1.  Choose the appropriate tab, depending on if you use YAML or JSON for your AppSpec file, and copy its content into the AppSpec file you just created\. For the `TaskDefinition` property, use the task definition ARN you noted in [ Step 1: Update your Amazon ECS application](tutorial-ecs-update-the-ecs-application.md) section\. 

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
               "ContainerName": "your-container-name",
               "ContainerPort": your-container-port
             }
           }
         }
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
             ContainerName: "your-container-name"
             ContainerPort: your-container-port
   ```

------
**Note**  
 Your replacement task set inherits the subnet, security group, platform version, and assigned public IP values from your original task set\. You can override these values for your replacement task set by setting their optional properties in your AppSpec file\. For more information, see [ AppSpec 'resources' section for Amazon ECS deployments ](reference-appspec-file-structure-resources.md#reference-appspec-file-structure-resources-ecs) and [ AppSpec File example for an Amazon ECS deployment ](reference-appspec-file-example.md#appspec-file-example-ecs)\. 

1.  Upload your AppSpec file to the S3 bucket you created as a prerequisite for this tutorial\. 