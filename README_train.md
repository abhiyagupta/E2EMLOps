# ML Model Training Pipeline
This repository contains a machine learning pipeline for training models on two datasets: sports and vegfruits. 

## Features

- Backend two models are trained - one for sports dataset and one for vegfruits dataset
- Dataset version control using DVC
- Metadata files stored in S3
- Docker container images pushed to ECR
- Training execution on EC2 instances triggered by git actions
- Automated training triggered by GitHub Actions
- Model artifacts saved to S3

## Setup

1. Clone repository 
``` 
git clone https://github.com/abhiyagupta/E2EMLOps.git 
cd E2MLOps
```

2. Install dependencies
```
pip install -r requirements.cpu.txt
```

3. Configure AWS credentials
```
sudo apt install update
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install
aws configure list
aws configure
```

5. Create a repository in ECR and bucket in s3 : AWS

- log on to aws console
- ECR: create a provate repo
- S3: create a bucket

6. Set up GitHub repository secrets for AWS access:

- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- AWS_REGION
- ECR_REPOSITORY_NAME
- S3_BUCKET_NAME
- S3_PREFIX
- PERSONAL_ACCESS_TOKEN : this is to be created in github developer settings. this gives access to run workflows. this will be like """ghp_********"""
- AWS_SUBNET_ID_1: go to VPC in aws, and then to subnet group. take any one subnet group avaiable. this will be like """subnet-******"""
- AWS_SECURITY_GROUP_ID: go to EC2, scroll down to Network and Security. Take the security group id. this will be like """sg-******"""

7. DVC Configuration

check DVC configuration:
```
dvc remote list 
aws s3 ls
```
NOTE: make sure raw data is stored under data/processed/sports and data/processed/vegfruits folder
data path sports: https://www.kaggle.com/datasets/gpiosenka/sports-classification
data path vegfruits: https://www.kaggle.com/datasets/kritikseth/fruit-and-vegetable-image-recognition/data

```
dvc add .\data\processed\vegfruits\
dvc add .\data\processed\sports\
dvc push
```
delete raw data - delete sports and vegfruits folder, but keep sprts.dc and vegfruits.dvc files created.
testing on local, this should pull data from s3: 
```
dvc pull
```

8. Manully run on local for testing. This will save artifacts to s3. 
```
dvc repro -f
```
9. Running on git actions:
- go to git actions
- start workflwo for train.yaml
- this will create an image, push to ECR
- start ec2 spot instance
- will pull data from s3
- train the models
- save all artifacts to s3
- stop the ec2 

