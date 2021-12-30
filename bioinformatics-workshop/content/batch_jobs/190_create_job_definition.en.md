---
title: "Create Job Definitions"
weight: 190
---

In this section we are going to create several job definitions. Job definitions
specify how jobs are to be run. While each job must reference a job definition, 
many of the parameters that are specified in the job definition can be 
overridden at runtime.

AWS Batch uses docker container images to run the jobs. We've created 
containers in the previous section (Building and registering containers) and 
registered them with the Elastic Container Registry (ECR). We are going to use 
these containers for the workshop. For each job definition we'll need to get 
their container image location first.

Use the AWS console search bar to find ECR and then click on Elastic Container 
Registry.

![find ecr](/static/images/batch_jobs/29_find_ecr.png)

You should be able to see the containers you've previously registered. Copy the 
fastqc container URI to be used for the job definition.

![find ecr](/static/images/batch_jobs/30_ecr_list.png)


Go back to the Batch console (you can search for it in the search bar), click 
on Job definitions on Batch left menu and then click on "Create".

![create job def button](/static/images/batch_jobs/31_create_job_def_button.png)

**Name** \- Give the job a name (e.g., biotech-fastqc).  
**Platform** \- Select EC2. We want to be able to choose which meachines we 
want to use and we can't do that with the Fargate option.  
**Job attempts** \- Set to 1 to only attempt to run this job once.  
**Execution timeout** \- Set to 600 to allow the job 10 minutes to run.

![create job def 1](/static/images/batch_jobs/32_create_job_def_1.png)

**Image** \- Paste the URI you've copied from ECR.  
**Command** \- Clear the content from the box. We are going to override the 
command when submitting the jobs.  
**vCPUs** \- Set to 1. For the fastqc command we only need 1 cpu. for other 
multi-threaded tools you might want to change this setting.  
**Memory** \- Set to 1000. For the demo purposes of this workshop 1000 MiB will 
be enough.

The CPU and Memory options can be overridden when submitting jobs in case we 
need a different amounts for these resources.

![create job def 2](/static/images/batch_jobs/33_create_job_def_2.png)

Expand the "Additional configuration" section.

**Job Role** \- Leave empty.  
**Execution Role** \- None.  
**Mount points** \- Add 2 mount points. `awscli` \- `/opt/aws-cli` and 
`data` \- `/data`. The awscli mount will allow the containers to run the 
aws cli without installing it locally (we installed it on the hosting machine 
using the launch template in the previous section).

![create job def 3](/static/images/batch_jobs/34_create_job_def_3.png)

**Volumes** \- Add 2 volumes to match the mount points. Name: `awscli`, Source 
path: `/opt/aws-cli` and Name: `data`, Source path: `/data`.

![create job def 4](/static/images/batch_jobs/35_create_job_def_4.png)

**Tags** \- Add the same tags as before (e.g., Environmnet - Dev, 
Project - Biotech). Also the "Enable" option for the Propogate Tags option.

When done, click on "Create".

![create job def 5](/static/images/batch_jobs/36_create_job_def_5.png)


Repeat this process for the other 3 biofinformatics tools we've registered with 
ECR (bwa, samtools, bcftools).


### bwa

**Name** \- biotech-bwa.  
**Platform** \- Select EC2.  
**Job attempts** \- Set to 1.  
**Execution timeout** \- Set to 1200.  
**Image** \- Paste the bwa URI from ECR.  
**Command** \- Clear the content from the box.  
**vCPUs** \- Set to 16.  
**Memory** \- Set to 32000.  
**Job Role** \- Leave empty.  
**Execution Role** \- None.  
**Mount points** \- Add 2 mount points. `awscli` \- `/opt/aws-cli` and 
`data` \- `/data`.  
**Volumes** \- Add 2 volumes to match the mount points. Name: `awscli`, Source 
path: `/opt/aws-cli` and Name: `data`, Source path: `/data`.  
**Tags** \- Add the same tags as before (e.g., Environmnet - Dev, 
Project - Biotech). Also the "Enable" option for the Propogate Tags option.


### samtools

**Name** \- biotech-samtools.  
**Platform** \- Select EC2.  
**Job attempts** \- Set to 1.  
**Execution timeout** \- Set to 300.  
**Image** \- Paste the bwa URI from ECR.  
**Command** \- Clear the content from the box.  
**vCPUs** \- Set to 4.  
**Memory** \- Set to 8000.  
**Job Role** \- Leave empty.  
**Execution Role** \- None.  
**Mount points** \- Add 2 mount points. `awscli` \- `/opt/aws-cli` and 
`data` \- `/data`.  
**Volumes** \- Add 2 volumes to match the mount points. Name: `awscli`, Source 
path: `/opt/aws-cli` and Name: `data`, Source path: `/data`.  
**Tags** \- Add the same tags as before (e.g., Environmnet - Dev, 
Project - Biotech). Also the "Enable" option for the Propogate Tags option.


### bcftools

**Name** \- biotech-bcftools.  
**Platform** \- Select EC2.  
**Job attempts** \- Set to 1.  
**Execution timeout** \- Set to 1200.  
**Image** \- Paste the bwa URI from ECR.  
**Command** \- Clear the content from the box.  
**vCPUs** \- Set to 2.  
**Memory** \- Set to 8000.  
**Job Role** \- Leave empty.  
**Execution Role** \- None.  
**Mount points** \- Add 2 mount points. `awscli` \- `/opt/aws-cli` and 
`data` \- `/data`.  
**Volumes** \- Add 2 volumes to match the mount points. Name: `awscli`, Source 
path: `/opt/aws-cli` and Name: `data`, Source path: `/data`.  
**Tags** \- Add the same tags as before (e.g., Environmnet - Dev, 
Project - Biotech). Also the "Enable" option for the Propogate Tags option.
