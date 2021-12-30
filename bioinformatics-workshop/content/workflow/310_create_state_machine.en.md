---
title: "Create a State Machine"
weight: 310
---

A state machine is a graphical representation of your workflow that you can use 
to examine the individual steps that define it.

We are going to use 
[Amazon States Language](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-amazon-states-language.html) 
to define the workflow. The Amazon States Language is a JSON-based, structured 
anguage used to define your state machine, a collection of states, that can do 
work (Task states), determine which states to transition to next 
(Choice states), stop an execution with an error (Fail states), and so on. 

Use the AWS console search bar to find step functions and then click on "Step 
Functions".

![find sfn](/static/images/workflow/02_find_step_functions.png)

On the left side menu click on "State machines".

![sfn menu](/static/images/workflow/03_step_functions_menu.png)

If the menu is not shown on the left side of the page, click on the menu button 
to open it.

![menu button](/static/images/workflow/04_menu_hamburger.png)

Click on the "Create state machine" button.

![create sfn button](/static/images/workflow/05_create_state_machine_button.png)

**Define state machine** \- Choose the "Write your workflow in code" option.  
**Type** \- Choose "Standard".

![create sfn1](/static/images/workflow/06_create_state_machine_1.png)

Copy the following code to the Definition section. Make sure the following are 
set correctly:  
Line 14 (jobQueue) - is the exact name you used for the Batch queue name.  
Line 16 - 19 - the job definition names from Batch with their current version.

Click "Next" when done.

:::code{showCopyAction=true showLineNumbers=true language=json}
{
  "Comment": "Bioinformatics workshop workflow",
  "StartAt": "Initialize",
  "States": {
    "Initialize": {
      "Type": "Pass",
      "InputPath": "$",
      "Parameters": {
        "workflow": {
          "name.$": "$$.StateMachine.Name",
          "execution.$": "$$.Execution.Name"
        },
        "params.$": "$.params",
        "jobQueue": "bioinformatics-default-queue",
        "jobdefs": {
          "fastqc": "biotech-fastqc:1",
          "bwa": "biotech-bwa:1",
          "samtools": "biotech-samtools:1",
          "bcftools": "biotech-bcftools:1"
        }
      },
      "Next": "FastQC"
    },
    "FastQC": {
      "Type": "Task",
      "InputPath": "$",
      "ResultPath": "$.result",
      "Resource": "arn:aws:states:::batch:submitJob.sync",
      "Parameters": {
        "JobName": "fastqc",
        "JobDefinition.$": "$.jobdefs.fastqc",
        "JobQueue.$": "$.jobQueue",
        "ContainerOverrides": {
          "Environment": [
            {
              "Name": "JOB_WORKFLOW_NAME",
              "Value.$": "$.workflow.name"
            },
            {
              "Name": "JOB_WORKFLOW_EXECUTION",
              "Value.$": "$.workflow.execution"
            },
            {
              "Name": "JOB_INPUTS",
              "Value": "${SAMPLES_PATH}"
            },
            {
              "Name": "SAMPLES_PATH",
              "Value.$": "$.params.environment.SAMPLES_PATH"
            },
            {
              "Name": "JOB_OUTPUTS",
              "Value": "*.html *.zip"
            },
            {
              "Name": "JOB_OUTPUT_PREFIX",
              "Value.$": "$.params.environment.JOB_OUTPUT_PREFIX"
            },
            {
              "Name": "SAMPLE_ID",
              "Value.$": "$.params.environment.SAMPLE_ID"
            }
          ],
          "Command": [
            "fastqc ${SAMPLE_ID}*.gz"
          ]
        }
      },
      "Next": "BwaMem"
    },
    "BwaMem": {
      "Type": "Task",
      "InputPath": "$",
      "ResultPath": "$.result",
      "Resource": "arn:aws:states:::batch:submitJob.sync",
      "Parameters": {
        "JobName": "bwa-mem",
        "JobDefinition.$": "$.jobdefs.bwa",
        "JobQueue.$": "$.jobQueue",
        "ContainerOverrides": {
          "Environment": [
            {
              "Name": "JOB_WORKFLOW_NAME",
              "Value.$": "$.workflow.name"
            },
            {
              "Name": "JOB_WORKFLOW_EXECUTION",
              "Value.$": "$.workflow.execution"
            },
            {
              "Name": "SAMPLES_PATH",
              "Value.$": "$.params.environment.SAMPLES_PATH"
            },
            {
              "Name": "REFERENCE_PATH",
              "Value.$": "$.params.environment.REFERENCE_PATH"
            },
            {
              "Name": "JOB_INPUTS",
              "Value": "${REFERENCE_PATH} ${SAMPLES_PATH}"
            },
            {
              "Name": "JOB_OUTPUTS",
              "Value": "*.sam"
            },
            {
              "Name": "JOB_OUTPUT_PREFIX",
              "Value.$": "$.params.environment.JOB_OUTPUT_PREFIX"
            },
            {
              "Name": "SAMPLE_ID",
              "Value.$": "$.params.environment.SAMPLE_ID"
            },
            {
              "Name": "REFERENCE_NAME",
              "Value.$": "$.params.environment.REFERENCE_NAME"
            }
          ],
          "Command": [
            "bwa mem -t 16 -p -o ${SAMPLE_ID}.sam ${REFERENCE_NAME}.fasta ${SAMPLE_ID}_*1*.fastq.gz"
          ]
        }
      },
      "Next": "Samtools"
    },
    "Samtools": {
      "Type": "Task",
      "InputPath": "$",
      "ResultPath": "$.result",
      "Resource": "arn:aws:states:::batch:submitJob.sync",
      "Parameters": {
        "JobName": "samtools-sort-index",
        "JobDefinition.$": "$.jobdefs.samtools",
        "JobQueue.$": "$.jobQueue",
        "ContainerOverrides": {
          "Environment": [
            {
              "Name": "JOB_WORKFLOW_NAME",
              "Value.$": "$.workflow.name"
            },
            {
              "Name": "JOB_WORKFLOW_EXECUTION",
              "Value.$": "$.workflow.execution"
            },
            {
              "Name": "JOB_INPUTS",
              "Value": "${JOB_OUTPUT_PREFIX}/${SAMPLE_ID}.sam"
            },
            {
              "Name": "JOB_OUTPUTS",
              "Value": "*.bam *.bai"
            },
            {
              "Name": "JOB_OUTPUT_PREFIX",
              "Value.$": "$.params.environment.JOB_OUTPUT_PREFIX"
            },
            {
              "Name": "SAMPLE_ID",
              "Value.$": "$.params.environment.SAMPLE_ID"
            }
          ],
          "Command": [
            "samtools sort -@ 4 -o ${SAMPLE_ID}.bam ${SAMPLE_ID}.sam; samtools index ${SAMPLE_ID}.bam"
          ]
        }
      },
      "Next": "CallVariantsByChromosome"
    },
    "CallVariantsByChromosome": {
      "Type": "Map",
      "InputPath": "$",
      "ItemsPath": "$.params.chromosomes",
      "MaxConcurrency": 23,
      "ResultPath": "$.result",
      "Parameters": {
        "workflow.$": "$.workflow",
        "params.$": "$.params",
        "chromosome.$": "$$.Map.Item.Value",
        "jobQueue.$": "$.jobQueue",
        "jobdefs.$": "$.jobdefs"
      },
      "Iterator": {
        "StartAt": "Bcftools",
        "States": {
          "Bcftools": {
            "Type": "Task",
            "InputPath": "$",
            "ResultPath": "$.result",
            "Resource": "arn:aws:states:::batch:submitJob.sync",
            "Parameters": {
              "JobName": "bcftools-mpileup-call",
              "JobDefinition.$": "$.jobdefs.bcftools",
              "JobQueue.$": "$.jobQueue",
              "ContainerOverrides": {
                "Environment": [
                  {
                    "Name": "JOB_WORKFLOW_NAME",
                    "Value.$": "$.workflow.name"
                  },
                  {
                    "Name": "JOB_WORKFLOW_EXECUTION",
                    "Value.$": "$.workflow.execution"
                  },
                  {
                    "Name": "REFERENCE_PATH",
                    "Value.$": "$.params.environment.REFERENCE_PATH"
                  },
                  {
                    "Name": "JOB_INPUTS",
                    "Value": "${REFERENCE_PATH} ${JOB_OUTPUT_PREFIX}/${SAMPLE_ID}.bam*"
                  },
                  {
                    "Name": "JOB_OUTPUTS",
                    "Value": "*.mpileup.gz *.vcf.gz"
                  },
                  {
                    "Name": "JOB_OUTPUT_PREFIX",
                    "Value.$": "$.params.environment.JOB_OUTPUT_PREFIX"
                  },
                  {
                    "Name": "SAMPLE_ID",
                    "Value.$": "$.params.environment.SAMPLE_ID"
                  },
                  {
                    "Name": "REFERENCE_NAME",
                    "Value.$": "$.params.environment.REFERENCE_NAME"
                  },
                  {
                    "Name": "CHROMOSOME",
                    "Value.$": "$.chromosome"
                  }
                ],
                "Command": [
                  "bcftools mpileup --threads 2 -r ${CHROMOSOME} -Oz -f ${REFERENCE_NAME}.fasta -o ${SAMPLE_ID}.${CHROMOSOME}.mpileup.gz ${SAMPLE_ID}.bam; bcftools call -m --threads 2 -t ${CHROMOSOME} -Oz -o ${SAMPLE_ID}.${CHROMOSOME}.vcf.gz ${SAMPLE_ID}.${CHROMOSOME}.mpileup.gz"
                ]
              }
            },
            "End": true
          }
        }
      },
      "End": true
    }
  }
}
:::

![create sfn2](/static/images/workflow/07_create_state_machine_2.png)

**Name** \- Give a name to the state machine (e.g., 
Bioinformatics-Workshop-Workflow).  
**Permissions** \- Choose "Create new role".

![create sfn3](/static/images/workflow/08_create_state_machine_3.png)

**Tags** \- Add the same tags as before (e.g., Environment - Dev, 
Project - Biotech).

When done, click on "Create state machine".

![create sfn4](/static/images/workflow/09_create_state_machine_4.png)