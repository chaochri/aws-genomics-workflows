---
title: "Setting up AWS Batch"
weight: 150
---

To use AWS batch we will need to set 3 components

[**Compute Environment**](https://docs.aws.amazon.com/batch/latest/userguide/compute_environments.html) \- 
Compute environments contain the Amazon ECS container instances that are used 
to run containerized batch jobs.

[**Job Queue**](https://docs.aws.amazon.com/batch/latest/userguide/job_queues.html) \- 
Jobs are submitted to a job queue where they reside until they can be scheduled 
to run in a compute environment.

[**Job Definition**](https://docs.aws.amazon.com/batch/latest/userguide/job_definitions.html) \- 
Job definitions specify how jobs are to be run. While each job must reference a 
job definition, many of the parameters that are specified in the job definition 
can be overridden at runtime.


To fully leverage the containers created by the tool in the previous section we 
would also want to change some of the default behaviors of the hosting machine
that is going to run our containers, such as volume size and default software. 
To do that we are going to create an 
[EC2 launch template](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-launch-templates.html) 
to be used by ECS on behalf of Batch.


