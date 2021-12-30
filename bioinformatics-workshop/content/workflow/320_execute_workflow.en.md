---
title: "Execute the workflow"
weight: 320
---

Now that we've defined a state machine with the bioinformatics workflow, we can 
test it with the same data we've previously used.

Navigate to the state machine you've just created (State Machines -> click on 
the name of the state machine).

Click on the "Start execution" button.

![start execution button](/static/images/workflow/10_start_execution_button.png)

Copy the following code to "Input" section. Change the ``BUCKET_NAME`` under the 
``JOB_OUTPUT_PREFIX`` parameter to the bucket you've previously created.

In this workflow we are going to use 2 chromosomes (chr19 & chr20) for the 
variant calling section to demonstrate how this step can be done in parallel.

When done, click on the "Start execution" button.

:::code{showCopyAction=true showLineNumbers=true language=json}
{
  "params": {
    "environment": {
      "REFERENCE_NAME": "Homo_sapiens_assembly38",
      "SAMPLE_ID": "SRR014820",
      "SAMPLES_PATH": "s3://1000genomes/pilot_data/data/NA12878/pilot3_unrecal/SRR014820_*.fastq.gz",
      "REFERENCE_PATH": "s3://broad-references/hg38/v0/Homo_sapiens_assembly38.fasta*",
      "REFERENCE_NAME": "Homo_sapiens_assembly38",
      "JOB_OUTPUT_PREFIX": "s3://BUCKET_NAME/output/SRR014820"
    },
    "chromosomes": [
      "chr19",
      "chr20"
    ]
  }
}
:::

![start execution](/static/images/workflow/11_start_execution.png)

You can track the workflow process on the execution page. A step will change 
colors depending on the status.
Green - step completed successfully.  
Blue - step is executing.  
Red - step failed.

![executing](/static/images/workflow/12_started_execution.png)

While the workflow is executing, you can navigate to AWS batch and see the jobs 
that were submitted by Step Functions.

When the workflow is successfully done, all the steps should be green and you 
can navigate to the S3 output folder to see the results.

![execution done](/static/images/workflow/13_workflow_done.png)