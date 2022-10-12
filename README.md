# AWS-healthAIhackathon
AWS Health AI Hackathon 2022 - ML classification model to support health recommendations

## Inspiration: 
Following up daily on how my father's Parkinson evolved over the last decade and how it impacted his day-to-day life was something that lead me to think of what AI could do to support health care industry. There is a tremendous amount of digital data about patients' conditions, medications and 
how the conditions evolve, and there are many people who would go through similar situations and if such information can be found it might be able to help another patient. 

During the cause I have searched about illnesses, symptoms, health conditions, nutrition, exercises related information on the internet, and I am sure many of you might have done that to help someone you love or for you. There are many digital resources available that can be used to create a knowledge base and use machine learning to learn from previous cases to help people with similar conditions.

## What it does: 
User can input information about health conditions, symptoms, prescriptions in a medical report to identify the health condition. The user input is run through the machine learning (ML) model which gives back the health classification that can be used to provide health recommendations on health professionals contacts, possible nutrition, exercises, therapies etc where health services can provide information to the user.

## How it is built: 
The user input is assumed to be uploaded on a web portal, which is then saved as a pdf file. This pdf file is passed through Comprehend Medical to identify the medical domain information in the text. I have trained the ML model with MTSamples(https://mtsamples.com/) transcription data related to some medical domains. Output of medical comprehend identified keywords are run against the trained ML classification model to identify the health domain & probability. The classification can be used by health care services to provide information and service recommendations.

## Technical implementation
1) Batch processing of medical text using Amazon Comprehend Medical: [HHBatchDataProcessing.ipynb](./HHBatchDataProcessing.ipynb)
!!! This code will query Comprehend Medical to extract medical related information of the text in the dataset and will cost around $100 to pass all records, therefore if you want to just test output reduce the calls!!! The processed file is saved under the data folder. 

2) Build, train and deploy a classification machine learning model with medical data extracted from the medical documents.: [HHModelDeployment.ipynb](./HHModelDeployment.ipynb)

## Used AWS services
- [Textract](https://aws.amazon.com/textract/): To extract text from the PDF medical report
- [Comprehend](https://aws.amazon.com/comprehend/): To process general language data from the output of Textract.
- [Comprehend Medical](https://aws.amazon.com/comprehend/medical/): To process medical-domain information from the output of Textract.
- [Sagemaker](https://aws.amazon.com/sagemaker/): Train the model using linear learner 
- [S3](https://aws.amazon.com/s3/): Store the data and the model

## Implementation and try out!

## Deploy the Working Environment

Deploy a Cloudformation template that will perform most of the initial setup.

1. Download the cloudformation template

Go to https://github.com/tde2020/AWS-healthAIhackathon/blob/main/aws-healthaihackathon.yaml, right click 'Save As' and download the cloudformation template.

**Note** Make sure you save the file as a .yaml file.

2. Create a new cloud formation stack

In another browser window or tab, login to your AWS account. Once you have done that, open the link below in a new tab to start the process of deploying the items you need via CloudFormation.

[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=HealthAIHackathon)

3. Upload the cloud formation template

Select `Upload a template`,  click `Choose file` and select the cloudformation template file you've just downloaded and then click `Next`.


4. Specify stack details

In this section, **optionally** specify the following options:
    
        1. Stack Name - Change the stack name to something more relevant if required.
        2. Notebook Name - Change the name of your SageMaker notebook which you will be using if required.
        3. Volume Size - Set the size of SageMaker EBS volume (default is 5GB). If you expect to load a larger dataset (i.e. if you want to reuse this lab to experiment with larger dataset), increase this accordingly.

When you're done, click the `Next` button at the bottom of the page.

5. Configure stack options

All of the defaults in this section will be sufficient to try out this setup. If you have any custom requirements, please alter as required. Once you're done, click the `Next` button to continue.

Finally, in the next section, scroll to the bottom of the page and check the checkbox to enable the template to create IAM resources and click the `Create stack` button.


It will take a few minutes to provision the resources required for the lab. Once it is completed, navigate to the `SageMaker` service by clicking `Services` in the top of the console and then search for `SageMaker` and click on the service.


6. Launch the SageMaker notebook

Click on Notebook instance and open the `aws-healthaihackathon` notebook (or the name of the notebook you provided in Cloudformation) by clicking `Open JupyterLab`

You can find the deployed files HHBatchDataProcessing.ipynb & HHModelDeployment.ipynb to walk through the code. 


## Cleanup
Once you're done with testing the code, please make sure you follow the instructions at the end of the notebook to delete all the resources you created in your AWS account. Once you have done with the cleanup, go to the **CloudFormation** service in the AWS console and delete the `HealthAIHackathon` stack. Please check manually on Sagemaker notebook, endpoints and S3 buckets to make sure all resources created are deleted, otherwise please delete them manually.

---

## References

AWS Comprehend Medical usage:
https://github.com/aws-samples/amazon-textract-and-comprehend-medical-document-processing

https://github.com/aws-samples/aws-healthcare-lifescience-ai-ml-sample-notebooks/blob/main/workshops/Process_HCLS_Docs_Using_AI_Services/Process-Medical-Documents.ipynb

AWS Linear Learner:
https://github.com/aws/amazon-sagemaker-examples/blob/main/scientific_details_of_algorithms/linear_learner_multiclass_classification/linear_learner_multiclass_classification.ipynb

Inspiration on dataset and text analysis:
https://www.kaggle.com/code/ritheshsreenivasan/clinical-text-classification/notebook

