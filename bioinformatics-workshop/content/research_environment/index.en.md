---
title: "Setting up a research environment"
weight: 10
---

## Overview

In this section of the workshop we are going to setup the basics of compute and 
storage needed to run bioinformatics workloads.

### Compute

To run the research and analysis tools we are going to need a virtual machine.
[Amazon Elastic Compute Cloud](https://aws.amazon.com/ec2/) (EC2) is a web 
service that provides secure, resizable compute capacity in the cloud. 
We are going to launch an EC2 instance, connect to it, install bioinformatics 
tools, and run them.

### Storage

There are several storage options on AWS and in this workshop we are going to 
work with [Amazon Simple Storage Service](https://aws.amazon.com/s3/) 
(Amazon S3) and [Amazon Elastic Block Store](https://aws.amazon.com/ebs/) 
(EBS).  
Amazon S3 is an object storage service that offers industry-leading 
scalability, data availability, security, and performance. We are going to use 
Amazon S3 as our centralize storage solution where we can store topology files 
or NextGen sequencing files. Files in Amazon S3 exists inside of buckets - we 
are going to create a bucket to store these files.  
EBS is an easy to use, high-performance, block-storage service designed for use 
with Amazon Elastic Compute Cloud. We are going to use EBS as our virtual 
machine storage for the operating system and for files we need to copy locally 
to the machine.

### Permissions

We will need to allow the Amazon EC2 instance to communicate with our Amazon S3 
bucket and for that we are going to use 
[AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM).  
IAM enables you to manage access to AWS services and resources securely. Using 
IAM, you can create and manage AWS users and groups, and use permissions to 
allow and deny their access to AWS resources. We are going to use 2 features of 
IAM to grant this permission, policies and roles.  
*IAM policy* is a document describing actions that are allowed or denied on 
specific AWS resources - for this example we are going to allow reading and 
writing content from an Amazon S3 bucket.  
*IAM roles* allow you to delegate access to users or services. We are going to 
assign a role to the Amazon EC2 instance and attach the Amazon S3 read/write 
permission policy.

### Connectivity

In order to install the tools and run them on an Amazon EC2 instance we are 
going to connect to the instance using an SSH client. For Linux and Mac users, 
this could be done via the terminal, and for Windows users this could be done 
using a software like [PuTTY](https://www.putty.org/).


## Estimated module length

45-60 minutes