---
title: AWS CDK - Python Lambda
date: 2022-06-22 13:00:00
tags:
- aws
- python
- github
- node
- docker
header_image: /intro/index-bg.jpg
categories:
  - cloud
---
Quick walk-though on how to publish a Python lambda that uses libraries not included in native Python container.
<!-- more -->

Read on to see the new [AWS Lambda Python module](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-lambda-python-alpha-readme.html#python-function) in action.

The example below uses TypeScript to describe a cloud environment and upload a Python lambda function. Why not use the Python AWS CDK? We certainly could, but I prefer TypeScript as it has more documentation examples online.


## Prerequisites

Follow all instructions on [cdkworkshop](https://cdkworkshop.com/) to setup AWS CLI, AWS CDK and NodeJS.

Install NPM with NVM. [Blog post here](https://nodesource.com/blog/installing-node-js-tutorial-using-nvm-on-mac-os-x-and-ubuntu/).
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
nvm install node
nvm use node
```

Install AWS CDK v2. [Full documentation here](https://docs.aws.amazon.com/cdk/v2/guide/getting_started.html).
```bash
npm install aws-cdk-lib
```

Setup AWS Account and user. [Guide here](https://cdkworkshop.com/15-prerequisites/200-account.html).
```bash
aws configure
```

Install Docker. [Full documentation here](https://docs.docker.com/desktop/mac/install/).
```
brew install docker
```

## Start

Create new directory and initialize with CDK.
```
mkdir ts_cdk_with_python_lambda
cd ts_cdk_with_python_lambda
cdk init app --language typescript
```

Install [Amazon Lambda Python Library](https://www.npmjs.com/package/@aws-cdk/aws-lambda-python-alpha).
```bash
npm i @aws-cdk/aws-lambda-python-alpha
npm install
```

Open `lib/ts_cdk_with_python_lambda-stack.ts` and add a new Python Function and associated API Gateway.

```js
import { Stack, StackProps } from 'aws-cdk-lib';
import { Construct } from 'constructs';
import { PythonFunction } from "@aws-cdk/aws-lambda-python-alpha";
import { aws_apigateway as apigw, aws_lambda as lambda } from "aws-cdk-lib";


export class TsCdkWithPythonLambdaStack extends Stack {
  constructor(scope: Construct, id: string, props?: StackProps) {
    super(scope, id, props);

    // Python Lambda Function
    const myPython = new PythonFunction(this, 'MyPythonHandler', {
      entry: './lambda_python',  // Folder containing lambda
      runtime: lambda.Runtime.PYTHON_3_9,  // Python version
      index: 'main.py',  // Name of Python file
      handler: 'handler'  // Name of method
    });

    // API Gateway
    new apigw.LambdaRestApi(this, 'MyPythonEndpoint', {
      handler: myPython
    })
  }
}

```

[Full documentation](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-lambda-python-alpha-readme.html#python-function) on PythonFunction.


Create new file `lambda_python/main.py` which will contain our lambda function. Notice how this file uses the `requests` library to make a HTTP GET request to an external web service.

```python
import requests

def handler(event, context):
    response = requests.get("https://api.ipify.org?format=json")
    print(response.text)
    return {
        'statusCode': 200,
        'headers': {
            'Content-Type': 'text/plain'
        },
        'body': f"You have hit {event['path']}\n"
                f"IP of lambda server: {response.json().get('ip', None)}"
    }
```

Create new requirements file `lambda_python/requirements.txt`
```
requests==2.28.0
```

Bootstrap your AWS account (if first time running AWS CDK). Then deploy!
```
cdk bootstrap
cdk diff
cdk deploy
```

Answer <yes> to prompts. If successful, you should see output with your new API URL.
```
Outputs:
TsCdkWithPythonLambdaStack.MyPythonEndpointABC = https://xyz.execute-api.us-west-2.amazonaws.com/prod/
```

Open the API Gateway URL in a browser and check for text output similar to below:
```
You have hit /
IP of lambda server: x.x.x.x
```

Congratulations on using the new AWS Lambda Python module!

## Bonus

Upload to GitHub.

```bash
git add .
git commit -m "python lambda"
git remote add origin https://github.com/<user>/ts_cdk_with_python_lambda.git
git push -u origin main
```

GitHub [ts_cdk_with_python_lambda](https://github.com/martyrsmith/ts_cdk_with_python_lambda) repo.

## References

* [cdkworkshop](https://cdkworkshop.com/)
* [@aws-cdk/aws-lambda-python-alpha module](https://docs.aws.amazon.com/cdk/api/v2/docs/aws-lambda-python-alpha-readme.html)
