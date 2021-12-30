---
title: "Building a bioinformatics workflow"
weight: 300
---

## Overview

In the previous sections of this workshop we discussed how to setup an 
environment for research using compute and storage, then we added a capability 
to submit jobs to AWS Batch so we can run more jobs. In this section we are 
going to expand on this knowledge and run bioinformatics pipelines at scale.

It is hard to run jobs at scale with running a single instance or submitting 
each manually. To run at scale we need a tool to orchestrate the workflow where 
jobs could run in parallel or sequential.

For example, we want to take sequencing files and first run QC, followed by 
alignment, then sort and index the output, and then use a variant calling tool.  
In this section of the workshop we are going to use all the tools we've 
previously created and defined in AWS Batch to run them as a full workflow.

![workflow](/static/images/workflow/01_workflow_diagram.png)

We are going to use [AWS Step Functions](https://aws.amazon.com/step-functions/) 
as the workflow manager.


AWS Step Functions is a low-code visual workflow service used to orchestrate 
AWS services, automate business processes, and build serverless applications. 
Workflows manage failures, retries, parallelization, service integrations, and 
observability so developers can focus on higher-value business logic.


## Estimated module length

25-30 minutes

