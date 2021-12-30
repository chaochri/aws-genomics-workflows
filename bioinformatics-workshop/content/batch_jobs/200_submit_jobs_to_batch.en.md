---
title: "Submit jobs to Batch"
weight: 200
---

In this section we are going to test the job definitions we've previously 
created. To test it we are going to use the aws cli through Cloud9.

Open your Cloud9 development environment you've created and change to the
batch_jobs code directory in the repo you've cloned.

:::code{showCopyAction=true showLineNumbers=true language=bash}
cd ~/environment/bioinformatics-workshop/code/batch_jobs
:::


### FastQC job

We are going to run a quality control for sample sequenced data (SRR014820).

Under the `code/batch_jobs/`, open the `fastqc.json` file and make sure the 
following properties are set correctly.

**jobQueue** \- The name of the queue as it's set in Batch. Navigate to AWS 
Batch -> Job queues to get the queue name.  
**jobDefinition** \- The name of the job definition as it's set in Batch. 
Navigate to AWS Batch -> Job definitions to get the job definition name.  
**Line 17 \- the value of the S3 bucket** \- Change "BUCKET_NAME" to the name 
of the S3 bucket you've previously created for this workshop.

When done, save the file and return to Cloud9 terminal window in the 
`bioinformatics-workshop/code/batch_jobs` folder.

Run the following command to submit a Batch job.

:::code{showCopyAction=true showLineNumbers=true language=bash}
aws batch submit-job --cli-input-json file://fastqc.json
:::

Navigate back to the Batch console and click on Jobs on the left menu. Choose 
the name of the queue you've previously created (e.g., 
bioinformatics-default-queue). You should see the submitted job on the list in 
a "RUNNABLE" status. This Will change to starting and running as Batch 
runs the job.

![batch runnable](/static/images/batch_jobs/37_job_runnable.png)

Click on the job name to get more details about the job. When the status changes 
to starting or running you'll see on the details page a link to a log stream. 
You can click on that link to see log details of the running jobs. The log has 
the information from the stdout of the running conatiner.

![batch running](/static/images/batch_jobs/38_job_running.png)

![batch logs](/static/images/batch_jobs/39_job_running_cloudwatch_logs.png)

When the job is completed you'll be able too see the output in the log 
indicating that the files were uploaded to S3.

![batch logs done](/static/images/batch_jobs/40_job_done_cloudwatch_logs.png)

You can now navigate to the S3 console and see the newly uploaded files.


We are going to repeat this process to the other tools we set up.


### BWA-MEM job

In this job we are going to run an alignment using sample sequenced data 
(SRR014820) and the human genome reference (Homo\_sapiens\_assembly38).

Under the `code/batch_jobs/`, open the `bwa-mem.json` file and make sure the 
following properties are set correctly.

**jobQueue** \- The name of the queue as it's set in Batch. Navigate to AWS 
Batch -> Job queues to get the queue name.  
**jobDefinition** \- The name of the job definition as it's set in Batch. 
Navigate to AWS Batch -> Job definitions to get the job definition name.  
**Line 17 \- the value of the S3 bucket** \- Change "BUCKET_NAME" to the name 
of the S3 bucket you've previously created for this workshop.

When done, save the file and return to Cloud9 terminal window in the 
`bioinformatics-workshop/code/batch_jobs` folder.

Run the following command to submit a Batch job.

:::code{showCopyAction=true showLineNumbers=true language=bash}
aws batch submit-job --cli-input-json file://bwa-mem.json
:::

You can now navigate back to the AWS Batch console and check the status of the 
job.

After this job is done we are going to use its ouptut for the next tool 
(samtools)


### Samtools-sort-index

We are going to use the data produced by the aligner in the previous job and 
run sort alignments by leftmost coordinates and then index the output bam file 
for fast access.

Under the `code/batch_jobs/`, open the `samtools-sort-index.json` file and make 
sure the following properties are set correctly.

**jobQueue** \- The name of the queue as it's set in Batch. Navigate to AWS 
Batch -> Job queues to get the queue name.  
**jobDefinition** \- The name of the job definition as it's set in Batch. 
Navigate to AWS Batch -> Job definitions to get the job definition name.  
**Lines 9 and 17 \- the value of the S3 bucket** \- Change "BUCKET_NAME" to the 
name of the S3 bucket you've previously created for this workshop.

When done, save the file and return to Cloud9 terminal window in the 
`bioinformatics-workshop/code/batch_jobs` folder.

Run the following command to submit a Batch job.

:::code{showCopyAction=true showLineNumbers=true language=bash}
aws batch submit-job --cli-input-json file://samtools-sort-index.json
:::

You can now navigate back to the AWS Batch console and check the status of the 
job.

After this job is done we are going to run the next tool (bcftools)


### Bcftools-mpileup-call

Generate VCF of containing genotype likelihoods. Run a variant calling on a 
chromosome (we are using chr19 for this task).

Under the `code/batch_jobs/`, open the `bcftools-mpileup-call.json` file and 
make sure the following properties are set correctly.

**jobQueue** \- The name of the queue as it's set in Batch. Navigate to AWS 
Batch -> Job queues to get the queue name.  
**jobDefinition** \- The name of the job definition as it's set in Batch. 
Navigate to AWS Batch -> Job definitions to get the job definition name.  
**Lines 9 and 17 \- the value of the S3 bucket** \- Change "BUCKET_NAME" to the 
name of the S3 bucket you've previously created for this workshop.

When done, save the file and return to Cloud9 terminal window in the 
`bioinformatics-workshop/code/batch_jobs` folder.

Run the following command to submit a Batch job.

:::code{showCopyAction=true showLineNumbers=true language=bash}
aws batch submit-job --cli-input-json file://bcftools-mpileup-call.json
:::

You can now navigate back to the AWS Batch console and check the status of the 
job.

When the job is done, you can navigate to S3 and see all the data that has been 
created by these tools.