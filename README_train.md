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

## Important to does:
- .github/workflows/01A_Deployment_Train_And_Store.yaml and .github\workflows\01D_Deployment_PR_Train_And_Store.yaml  :  POINT TO YOUR REMOTE IN DVC PULL. HERE REMOTE IN .dvc/config is "mlops"
- image to be used : ami-01654480b8a1994bd  (image id in ec2) 
- github\workflows\01A_Deployment_Train_And_Store.yaml : edit docker_username &   docker_password to point to your ECR
- src/backend/torch_local/transfer_mar.py : change bucket name to point to your bucket line no 
57.
- 01D: s3 buckt name point to your bucker - line no 236 
- 01B: s3 buckt name point to your bucker - line no 660 
- 01B point to ur path line 334
- 01B argo cd: change repo name and point to correct argocd repo

## Setup

1. Clone repository 
``` 
git clone https://github.com/abhiyagupta/E2EMLOps.git 
cd E2MLOps
```

2. Install dependencies
```
pip install -r requirements.gpu.txt
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
- AWS_ECR_LOGIN_URI : 123456789012.dkr.ecr.us-east-1.amazonaws.com  -- where 123456789012 is aws account id
- CDK_ACCOUNT: aws 12 digit account id -- 123456789012 

7. first run manually from local to push train prod and dev model to s3.
- launch instance
- purchasing: spot
- custmize spot instance: request type-persistent, interuption behavious: stop
- start ec2 - g4dn x large
- image: ami-01654480b8a1994bd
- size -128 GB
- configure vs code to ec2
- follow the commands below in ec2 machine

8. 
```
git clone https://github.com/abhiyagupta/E2EMLOps.git
python -m venv venv
source venv/bin/activate
pip install -r requirements.gpu.txt 
```


9.  DVC Configuration (first time local)

check DVC configuration:
```
dvc init
dvc remote list 
aws s3 ls 
```
- this will create dvc config file
```
dvc remote add -d mlops s3://abhiya-mlops-project/data
```
NOTE: make sure raw data is stored under data/processed/sports and data/processed/vegfruits folder
data path sports: https://www.kaggle.com/datasets/gpiosenka/sports-classification
data path vegfruits: https://www.kaggle.com/datasets/kritikseth/fruit-and-vegetable-image-recognition/data

```
dvc add .\data\processed\vegfruits\
dvc add .\data\processed\sports\
dvc push
```
delete raw data - delete sports and vegfruits folder, but keep sports.dc and vegfruits.dvc files created.
testing on local, this should pull data from s3: 
```
dvc pull - r mlops  
```

8. Manully run on local for testing. This will save artifacts to s3. 
```
dvc repro -f
```

9. manully first save models to s3 : prod and dev mode
```
python src/backend/torch_local/transfer_mar.py -p prod
python src/backend/torch_local/transfer_mar.py -p dev
```

10. Running on git actions:
- go to git actions
- start workflow for ".github\workflows\01C_Deployment_PR.yaml"
- this will create an image, push to ECR
- start ec2 spot instance
- will pull data from s3
- train the models
- save all artifacts to s3
- stop the ec2 

