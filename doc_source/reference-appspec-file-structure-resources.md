# AppSpec 'resources' Section \(AWS Lambda Deployments Only\)<a name="reference-appspec-file-structure-resources"></a>

The 'resources' section specifies the Lambda function to deploy and has the following structure:

YAML:

```
resources:
  - name-of-function-to-deploy:
      type: "AWS::Lambda::Function"
      properties:
        name: name-of-lambda-function-to-deploy
        alias: alias-of-lambda-function-to-deploy
        currentversion: version-of-the-lambda-function-traffic-currently-points-to
        targetversion: version-of-the-lambda-function-to-deploy
```

JSON:

```
"resources": [{
	"name-of-function-to-deploy": {
		"type": "AWS::Lambda::Function",
		"properties": {
			"name": "name-of-function-to-deploy",
			"alias": "alias-of-lambda-function-to-deploy",
			"currentversion": "version-of-the-lambda-function-traffic-currently-points-to",
			"targetversion": "version-of-the-lambda-function-to-deploy"
		}
	}
}]

```

The instructions are as follows:

+ **name** – Required\. This the name of the Lambda function to deploy\.

+ **alias** – Required\. This is the name of the alias to the Lambda function\.

+ **currentversion** – Required\. This is the version of the Lambda function traffic currently points to\.

+ **targetversion** – Required\. This is the version of the Lambda function traffic is shifted to\.
