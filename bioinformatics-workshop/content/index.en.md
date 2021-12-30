---
title: "Bioinformatics on AWS Workshops"
weight: 0
---

## Intro

When running bioinformatics workloads in the cloud, customers usually need a 
research environment to test different tools and write custom code, the ability 
to run these tools and code at scale, and also the ability to chain these 
tools and code for a full pipeline.

This workshop is intended to bioinformatics scientists and engineers that are 
required to build and maintain bioinformatics tools and pipeplines in the cloud.

In this workshop you'll learn how to create a bioinformatics research 
environment, install and test tools, run these tools at scale, and build a 
genomics variant calling pipeline.


### Bioinformatics tools

Throught the workshop we are going to use the following bioinformatics tools:

**FastQC** \- A quality control tool for high throughput sequence data.  
**BWA** \- Burrows-Wheeler Aligner for aligning short sequence reads to a 
reference genome.  
**samtools** \- Sequence Alignment Mapping library for indexing and sorting 
aligned reads.  
**bcftools** \- Binary (V)ariant Call Format library for determining variants 
in sample reads relative to a reference genome.


## The workshop modules

### Setting up a research environment

In this module you will be establishing the foundation needed to run 
bioinformatics workloads. You will create an Amazon Elastic Comute Cloud 
instance (EC2) which is a virtual machine, Amazon S3 bucket (storage), and 
granting permissions to the EC2 instance to work with the S3 bucket. Then you 
are going to connect to the EC2 instance, install bioinformatics tools, test 
them, and store the results in the S3 bucket. You are also going to run AWS 
Cloud9 as a convinient way to run a machine, write code, and execute terminal 
commands.


### Running Bioinformatics Jobs

In this module you are going to build a system where we can submit 
bioinformatics jobs. You are going to create an Amazon VPC (network setup), 
learn how to use Docker containers, setup AWS Batch to run bioinformatics jobs, 
and test them.


### Building a bioinformatics workflow
In this module you are going to use AWS Step Functions to create a full 
bioinformatics pipeline workflow.

::alert[The modules in this workshop are based on top of each other and you must complete them in order]

You can read more about the base for this workshop [here](https://BLOG=POST)


## Prerequisites

1. AWS Account. If you don't have an account already, [create one here](https://aws.amazon.com/getting-started/)
2. You must have administrator access to your AWS account.
3. You are comfortable running commands in a terminal window.


## Costs

In this workshop you are going to create AWS resources that have cost. Follow 
the [clean up guide](500-cleanup) when done with this workshop to avoid 
incurring unnecessary costs.


## Estimated workshop length

2 - 3 hours