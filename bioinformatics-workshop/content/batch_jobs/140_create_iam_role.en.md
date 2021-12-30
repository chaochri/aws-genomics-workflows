---
title: "Create an IAM role"
weight: 140
---

AWS Batch is using Amazon ECS to run jobs and we need to give the ECS tasks 
(the jobs containers we are going to run) permission to S3 in the same way we 
gave permissions to an EC2 instance in the first section of this workshop 
(Compute and Storage).

Use the AWS console search bar to find roles and then click on Roles.

![find roles](/static/images/batch_jobs/01_role_search.png)

In the left menu click on Roles and then click on the "Create role" button.

![create role button](/static/images/batch_jobs/02_create_role_button.png)

**Select type of trusted entity** \- Select "AWS service".  
**Choose a use case** \- Select "Elastic Container Service"

![create role 1](/static/images/batch_jobs/03_create_role_1.png)

**Select your use case** \- Select "EC2 Role for Elastic Container Service".

![create role 2](/static/images/batch_jobs/04_create_role_2.png)

When done, click on "Next: Permissions".

The `AmazonEC2ContainerServiceForEC2Role` policy should be added automatically.

Click on "Next: Tags".

![create role 3](/static/images/batch_jobs/05_create_role_3.png)

You can skip the tags creation for the role and click on "Next: Review".

**Role name** \- Give the role a name (e.g., biotech-jobs-ecs).  
**Role description** \- Optionally describe the role.

When done, click on "Create role".

![create role 4](/static/images/batch_jobs/06_create_role_4.png)

Search for the newly created role (e.g., biotech-jobs-ecs) and click on the 
role name.

![create role 5](/static/images/batch_jobs/07_create_role_5.png)

Click on Attach policies.

![create role 6](/static/images/batch_jobs/08_create_role_6.png)

Search for the policy you've previously created to allow access to S3 
(e.g., biotech-research-bucket-read-write), check the checkbox next to the 
policy name and click on "Attach policy".

![create role 7](/static/images/batch_jobs/09_create_role_7.png)
