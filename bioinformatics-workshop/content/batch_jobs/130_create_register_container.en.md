---
title: "Building and registering containers"
weight: 130
---

After you create a Dockerfile, you’ll need to build it (create an image) and 
register it with a docker registry.

In the code you've previously cloned with Cloud9, there’s a tool that can help 
you build and register the containers in Amazon ECR. 
Read more about using this tool 
[here](https://github.com/aws-samples/aws-genomics-workflows/tree/master/src/aws-genomics-cdk/containers).

The following instructions will guide you how to use the tool to register 
containers.

The tool is relying on 2 environment variables ``CDK_DEFAULT_ACCOUNT`` 
(your AWS 12 digit account id) and ``CDK_DEFAULT_REGION`` 
(the region where you would like to deploy your containers. e.g., us-west-2).

Navigate to the Cloud9 environment and open a terminal window.

Run the following commands to add these to the environemnt variables. Change 
the region to your preferred region and set your AWS account id

:::code{showCopyAction=true showLineNumbers=true language=bash}
export CDK_DEFAULT_REGION='us-west-2'
export CDK_DEFAULT_ACCOUNT='111111111111'
:::

To build and register the bioinformatics containers for this workshop navigate 
to the containers directory.

:::code{showCopyAction=true showLineNumbers=true language=bash}
cd ~/environment/bioinformatics-workshop/code/containers
:::

You can build and register one or more of the containers already present in the 
folder, such as bwa, fastqc, minimap2, etc.

We are going to build and register all the containers in the folder to be used 
later on in this workshop. Run the following commands to build and register the 
containers.

:::code{showCopyAction=true showLineNumbers=true language=bash}
chmod +x build.sh
./build.sh fastqc biotech
./build.sh bwa biotech
./build.sh bcftools biotech
./build.sh samtools biotech
:::





