# Create a deployment configuration with CodeDeploy<a name="deployment-configurations-create"></a>

You can use the CodeDeploy console, AWS CLI, the CodeDeploy APIs, or an AWS CloudFormation template to create custom deployment configurations\. 

For information about using an AWS CloudFormation template to create a deployment configuration, see [AWS CloudFormation templates for CodeDeploy reference](reference-cloudformation-templates.md)\.

To use the AWS CLI to create a deployment configuration, call the [create\-deployment\-config](https://docs.aws.amazon.com/cli/latest/reference/deploy/create-deployment-config.html) command, specifying:
+ A name that uniquely identifies the deployment configuration\. This name must be unique across all of the deployment configurations you create with CodeDeploy associated with your AWS account\.
+ The minimum number or percentage of healthy instances that should be available at any time during the deployment\. For more information, see [CodeDeploy instance health](instances-health.md)\.

The following example creates an EC2/On\-Premises deployment configuration named ThreeQuartersHealthy that requires 75% of target instances to remain healthy during a deployment:

```
aws deploy create-deployment-config --deployment-config-name ThreeQuartersHealthy --minimum-healthy-hosts type=FLEET_PERCENT,value=75
```

The following example creates an AWS Lambda deployment configuration named Canary25Percent45Minutes\. It uses canary traffic shifting to shift 25 percent of traffic in the first increment\. The remaining 75 percent shifted 45 minutes later:

```
aws deploy create-deployment-config --deployment-config-name Canary25Percent45Minutes --traffic-routing-config "type="TimeBasedCanary",timeBasedCanary={canaryPercentage=25,canaryInterval=45}" --compute-platform Lambda
```

The following example creates an Amazon ECS deployment configuration named Canary25Percent45Minutes\. It uses canary traffic shifting to shift 25 percent of traffic in the first increment\. The remaining 75 percent shifted 45 minutes later:

```
aws deploy create-deployment-config --deployment-config-name Canary25Percent45Minutes --traffic-routing-config "type="TimeBasedCanary",timeBasedCanary={canaryPercentage=25,canaryInterval=45}" --compute-platform ECS
```