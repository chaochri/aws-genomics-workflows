---
title: "Running Bioinformatics Jobs"
weight: 100
---

Using a managed environment with installed bioinformatics tools, such as EC2 or
Cloud9, is a convenient way to run bioinformatics tools for research purposes 
but not so much for running these tools as parallel multiple jobs and 
automating the process.

Instead of running the bioinformatics tools in a virtual machine, we are going 
to containerize them using Docker containers and then use AWS Batch to run them.

[AWS Batch](https://aws.amazon.com/batch/) enables developers, scientists, and 
engineers to easily and efficiently run hundreds of thousands of batch 
computing jobs on AWS. AWS Batch dynamically provisions the optimal quantity 
and type of compute resources (e.g., CPU or memory optimized instances) based 
on the volume and specific resource requirements of the batch jobs submitted.

In this section of the workshop we are going to create a network setup with 
best practices to run Batch jobs, create bioinformatics containers, setup AWS 
Batch, and test these jobs.


## Networking

AWS global infrastructure is built in multiple geographic locations across the 
world called regions. Each one of the regions has multiple clusters of data 
centers called availability zones (AZs). AZs give customers the ability to 
operate production applications and databases that are more highly available, 
fault tolerant, and scalable than would be possible from a single data center. 
All AZs in an AWS Region are interconnected with high-bandwidth, low-latency 
networking, over fully redundant, dedicated metro fiber providing 
high-throughput, low-latency networking between AZs. 
[Learn more about regions and availability zones](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/).

When launching compute resources, such as EC2 instance, we need to provide the 
networking in which the resources will reside. Amazon Virtual Private Cloud 
(Amazon VPC) is a service that lets you launch AWS resources in a logically 
isolated virtual network that you define.

Every region has a default VPC (we used this in the previous sections of the 
workshop) which is convenient for deploying and connecting to resources.  
The default VPC comes with multiple public subnets; a public subnet means that 
there’s a route from the internet to the resources which also means that it’s 
possible for unauthorized entities to try and gain access to the resources in 
these subnets. To protect our resources we will create a custom VPC with both 
public and private subnets (a private subnet doesn’t have a direct route from 
the internet). 
[Learn more about setting up a VPC with public and private subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario2.html).

We are going to run the Batch jobs in private subnets.


## Permissions

In the previous section of the workshop we needed to give EC2 access to work 
with S3. We will be creating another IAM role in this section to allow Batch to 
work with S3.


## Docker Containers

Docker can package an application and its dependencies in a virtual container 
that can run on any Linux, Windows, or macOS computer. Learn more about 
[Docker](https://en.wikipedia.org/wiki/Docker_(software)).

We are going to create several docker container with bioinformatics tools.  
**FastQC** \- A quality control tool for high throughput sequence data.  
**BWA** \- Burrows-Wheeler Aligner for aligning short sequence reads to a 
reference genome.  
**samtools** \- Sequence Alignment Mapping library for indexing and sorting 
aligned reads.  
**bcftools** \- Binary (V)ariant Call Format library for determining variants 
in sample reads relative to a reference genome.

We are going to build these containers and register them with Amazon ECR.

[Amazon Elastic Container Registry](https://aws.amazon.com/ecr/) 
(Amazon ECR) is a fully managed container registry that makes it easy to store, 
manage, share, and deploy your container images and artifacts anywhere. Amazon 
ECR eliminates the need to operate your own container repositories or worry 
about scaling the underlying infrastructure.



## Cloud9

We are going to use the Cloud9 environment you've previously created to run 
bioinformatics tools from this workshop to buils and register containers. We 
are also going to use the AWS CLI to create the networking environment (VPC) 
and submit jobs to AWS Batch.


## Estimated module length

60-90 minutes