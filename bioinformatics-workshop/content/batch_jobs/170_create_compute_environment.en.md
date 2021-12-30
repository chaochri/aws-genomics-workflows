---
title: "Create a Compute Environment"
weight: 170
---

Use the AWS console search bar to find Batch and then click on "Batch".

![find Batch](/static/images/batch_jobs/16_find_batch.png)

On the left menu click on "Compute environments".

![Batch menu](/static/images/batch_jobs/17_batch_menu.png)

Click on "Create".

![Create env button](/static/images/batch_jobs/18_create_compute_env_button.png)


**Compute environment type** \- Select "Managed". This will make sure AWS will 
scale and configure the instances on your behalf.  
**Compute environment name** \- Give a name to the compute environment 
(e.g., bioinformatics-default).  
**Enabled compute environment** \- Leave as checked.  
**Service role** \- Select "Batch service-linked role".

Change the **"Provisioning model"** to Spot and the "Additional settings: 
service role, instance role, EC2 key pair" section will appear above. You can 
learn more about Spot instances [here](https://aws.amazon.com/ec2/spot/).  
Expand the "Additional settings: service role, instance role, EC2 key pair" 
section.

**Service Role** \- Batch service-link role.
**Instance Role** \- select the role you've previously created to be used by 
batch. (e.g., biotech-jobs-ecs).  
**EC2 keypair** \- We are not going to connect directly to the EC2 machines so 
you can leave this empty.

![Create env 1](/static/images/batch_jobs/19_create_compute_env_1.png)

**Maximun % on-demand price** \- 100.  
**Minimum vCPUs** \- 0. This will ensure to scale the running instances back to 
0 when there are no jobs to process.  
**Maximun vCPUs** \- 256. This will be the upper limit of the cpus in all the 
running instances and can help control usage and cost.  
**Desired vCPUs** \- 0. Decide how many vCPUs we need when we create the compute 
environment. We don't have any jobs that need to run right now so we can keep 
it at 0.

![Create env 2](/static/images/batch_jobs/20_create_compute_env_2.png)

**Allowed instance types** \- Remove the "optimal" option if it is checked and 
add the c5 family. This will allow Batch to luanch only instances from the c5 
family (Amazon EC2 C5 instances deliver cost-effective high performance at a 
low price per compute ratio for running advanced compute-intensive workloads). 
Learn more about [Amazon EC2 C5 instances](https://aws.amazon.com/ec2/instance-types/c5/).  
**Allocation strategy** \- Select "SPOT\_CAPACITY\_OPTIMIZED".

Exapnd the "Additional settings: launch template, user specific AMI" section.  
**Launch template** \- Select the launch template you've created in the previous 
section.

![Create env 3](/static/images/batch_jobs/21_create_compute_env_3.png)

**VPC ID** \- Select the VPC you've previously created with the CDK deployment.  
**Subnets** \- Keep only the subnets that are labled "Private". This will insure 
that Batch will launch instances in private subnets.  

![Create env 4](/static/images/batch_jobs/22_create_compute_env_4.png)

**EC2 tags** \- Add the same tags we've previously used for EC2 
(e.g., Name - biotech-job-batch, Environment - Dev, Project - Biotech).  

When done click on "Create compute environment".

![Create env 5](/static/images/batch_jobs/23_create_compute_env_5.png)

The compute environment may take a while to be created and the status of it 
will change from "Creating" to "Valid" when done. When the status change to 
"Valid" continue to the next section - "Create a job queue".

![Creating env](/static/images/batch_jobs/24_creating_compute_env.png)
