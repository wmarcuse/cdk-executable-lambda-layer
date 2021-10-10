# CDK executable in AWS Lambda

![Apache Groovy](https://img.shields.io/badge/Apache%20Groovy-4298B8.svg?style=for-the-badge&logo=Apache+Groovy&logoColor=white) ![Gradle](https://img.shields.io/badge/Gradle-02303A.svg?style=for-the-badge&logo=Gradle&logoColor=white)

This repository shows how to add a [CDK](https://aws.amazon.com/cdk/) executable to a NodjeJS runtime for [AWS Lambda](https://aws.amazon.com/lambda/). It utilizes Lambda Layer to provide CDK to the lambda.

## Prerequisites

- [Java JDK 8+](https://adoptopenjdk.net/)
- [Gradle](https://gradle.org/install)

## Usage
- Clone the repository to your machine
- Export Lambda Layer Zip
  - `gradle -q exportLambdaLayerZip`

Now the lambda layer zip is available in `/build/layer/aws-cdk.zip`

Use the Zip file to create a new Lambda Layer Version i.e. via the Web Console, CDK or in another way.

Then create a Lambda function with the Layer attached and the following handler assigned with a NodeJs runtime:

    exports.handler = async (event) => {
      const spawnSync = require('child_process').spawnSync;
      const process = spawnSync('cdk', ['--version'], {
      stdio: 'pipe',
      stderr: 'pipe'
      });
      console.log(process.status);
      console.log(process.stdout.toString());
    };

---
**NOTE**

This project is tested under Linux and Windows WSL-2. There may be executable issues when building the layer via a pure Windows cli.

---
