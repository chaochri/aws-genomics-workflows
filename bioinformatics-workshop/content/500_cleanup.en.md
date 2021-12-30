---
title: "Cleanup guide"
weight: 500
---

This guide will help you cleanup the resources you've created this workshop in 
order to avoid any extra charges in the future.

::alert[If the AWS account you've used for this workshop is being used for other workloads, pay attention to only delete resources created for this workshop]


## Step Functions

There's no cost for Step Functions while it's not executing. You can elect to 
keep the state machine if you like or remove it with the following instructions.

Navigate to the Step Functions console, click on "State machines" on the left 
menu, select the state machine you've created, and then click on the "Delete" 
button.

![delete button](/static/images/cleanup/01_delete_button.png)

Confirm deleting the state machine by clicking on the "Delete state machine" 
button.

![delete sfn](/static/images/cleanup/02_delete_state_machine.png)


## Batch

There are no costs attached with AWS Batch while nothing is being executed 
unless you've set the Compute Environment to have more the the minimum of 0 
vCPUS. In the workshop we suggested to set it to 0 so no machines will be 
running if there are no tasks to execute.
You can keep the Batch configuration as is if you like or delete it following 
these instructions.

Navigate to Batch console, click on "Job definitions" on the left menu. Choose 
a job definition from the list and click on the "Deregister" button.

![deregister button](/static/images/cleanup/03_deregister_button.png)

Confirm deregistering the job definition by clicking on the "Deregister job 
definition" button.

![deregister confirm](/static/images/cleanup/04_deregister_job_definition.png)

Repeat this process for the remaining job definitions.

Click on "Job queues" on the left menu, click on the radio button for the queue 
you've previously created and click on the "Disable" button.

![disable button](/static/images/cleanup/05_disable_button.png)

Wait a few moments for the queue status to change to disabled, then click 
again on the radio button for the queue, and click on the "Delete" button.

![delete button](/static/images/cleanup/01_delete_button.png)

Confirm deleting the queue by clicking on "Delete job queue".

![delete job queue](/static/images/cleanup/06_delete_job_queue.png)

Click on "Compute environments" on the left menu, click the radio button for 
the compute environment you've previously created, and then click on the 
"Disable" button.

![disable button](/static/images/cleanup/05_disable_button.png)

Wait few moments for the compute environment state to change to disabled, then 
click again on the radio button for the compute environment, and click on the 
"Delete" button.

![delete button](/static/images/cleanup/01_delete_button.png)

Confirm deleting the compute environment by clicking on "Delete compute 
environment".


## Launch Template

There's no cost for keeping the launch template and you can delete it by 
following these instructions.

Navigating to the EC2 console and click on "Launch Templates" on the left menu.
Click the radio button next to the launch template you've previously created, 
click on the "Actions button", and then click on "Delete template".

![delete template button](/static/images/cleanup/08_delete_launch_template_button.png)

Confirm deleting the template by typing "Delete" in the confirm box and then 
click on "Delete".

![delete template](/static/images/cleanup/09_delete_launch_template.png)


## ECR

You can keep the container images you've previously created if you like.
ECR Storage is $0.10 per GB / month for data stored in private or public 
repositories.
To delete the container images from ECR, follow these instructions.

Navigate to the ECR console, check the radio button next to one of the images 
you've previously registered with ECS, and then click on the "Delete" button.

![delete button](/static/images/cleanup/01_delete_button.png)

Confirm deleting the container image by typing "delete" in the input box and 
then click on "Delete".

![delete container](/static/images/cleanup/10_delete_container_image.png)

Repeat this process for all the other container images you've registered in 
this workshop.


## VPC

You can keep the VPC you've previously created but some parts of that VPC will 
have associated costs, such as NAT Gateways.You can delete the VPC by following 
these instructions.

Navigate to you Cloud9 environment, open a terminal window, and navigate to the 
vpc folder.

:::code{showCopyAction=true showLineNumbers=true language=bash}
cd ~/environment/bioinformatics-workshop/code/vpc
:::

Delete the vpc
:::code{showCopyAction=true showLineNumbers=true language=bash}
cdk destroy
:::

When prompted "Are you sure you want to delete: VpcStack (y/n)?", type "y" and 
press the Enter key.

This will take few minutes to complete.


## Cloud9

Cloud9 will cost compute time while it's running and storage reagrdless if it's 
running or not. You can keep the Cloud9 environment if you like or delete it 
following these instructions.

If you hava a browser tab open with Cloud9, then close it first. Navigate to 
the Cloud9 console (you can use the search bar to find it) and click on "Your 
environments" on the left menu. Select the Cloud9 environment you've previously 
created and then click on the "Delete" button.

![delete button](/static/images/cleanup/01_delete_button.png)

Confirm deleting the Cloud9 environment by typing "Delete" in the input box and 
then click on "delete".


## EC2

The same as Cloud9, EC2 will cost compute time while it's running and storage 
regardless if it's running or not. You can keep the EC2 instance if you like or 
delete it following these instructions.

Navigate to the EC2 console and click on "Instances" on the left menu. Check 
the box next to the EC2 instance you've previously created, then click on the 
"Instance state" button, and click on "Terminate instance".

![terminate instance button](/static/images/cleanup/12_terminate_instance_button.png)

Confirm terminating the instance by clicking on "Terminate".

![terminate instance](/static/images/cleanup/13_terminate_instance.png)


## S3

S3 will cost by the storage space used. You can keep the files you've crated 
through this workshop or delete them and the bucket by following these 
instructions.

Navigate to the S3 console, check the radio button next to the bucket you've 
previously created, and then click on the "Empty" button.

![empty button](/static/images/cleanup/14_empty_button.png)

Confirm deleting all the files in the bucket by typing "permanently delete" in 
the input box and then click on "Empty".

![empty bucket](/static/images/cleanup/15_empty_bucket.png)

After all files have been deleted, click on the "Exit" button.

![exit button](/static/images/cleanup/16_exit_button.png)

Select the same bucket again and click on the "Delete" button.

![delete button](/static/images/cleanup/01_delete_button.png)

Confirm deleting the bucket by typing the bucket name in the input box and then 
click on "Delete bucket".


## IAM

IAM has no associated costs. You can keep the policies and roles or delete them.  
If you decided to keep any of the previous services, some or all the policies 
and roles you've created will be required to keep these services working 
properly.

To delete the IAM resources you've created, follow these instructions.

Navigate to the IAM console and click on "Roles" on the left menu. Type "Bio" 
in the search bar to find related roles to this workshop, select all the roles 
you wish to delete and then click on the "Delete" button.

![search delete roles](/static/images/cleanup/18_roles_search.png)

Confirm deleting these roles by typing "delete" and then click on "Delete".

![delete roles](/static/images/cleanup/19_delete_roles.png)

Click on "Policies" on the left menu and search for "biotech" (or the name of 
the policy you've previously created).

![policy search](/static/images/cleanup/20_policies_search.png)

Click on the radio button next to the policy you would like to delete, then 
click on the "Actions" button and select "Delete".

![policy search](/static/images/cleanup/21_delete_policy_button.png)

Confirm deleting the policy by typing the policy name in the input box and then 
click on "Delete".