aws Lambda deployemnt on local:
- NOTE: make sure LambdaFn/download_files bucket name is pointing to your s3 bucket

```
cd LambdaFn/
sudo apt install npm
sudo npm install -g aws-cdk
cdk --version
```

Configure AWS
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install
aws configure 
```

create virtual environment
```
python -m venv .venv
source .venv/bin/activate
```

install requirements:
```
python -m pip install -r aws-req.txt
pip install -r requirements.lambda.txt 
```

Download the trained model from s3: 
- this will create a s3_files files foldler inside LambdaFn folder
```
python download_files.py 
```

```
python app.py
```

ONLY for LOCAL: point to ur aws id (via terminal)
- not required for git actions, already added in secrets
- command below edit region as per your aws account region

```
export CDK_DEFAULT_ACCOUNT=*12 digit aws acc id*
cdk bootstrap aws://*12 digit aws acc id*/ap-south-1
cdk deploy --require-approval=never
```
- this will create : CHECK AWS CONSOLE FOR URL
- cdktoolkit & gradiolambdafn stacks in AWS cloud formation
- in lambda, this will create a emlolambdafunction
- aws-->lambda--> manage
- in lambda function, check function url.
- paste function url in chrome to see deployed end result
- remember, for aws lambda it is cold start, so it will tke some time for URL to load

destroy/stop
```
cdk destroy
```

GIT ACTIONS:
- run Deployment 03 Lambda Deployment with Gradio
- after reaching deplaoy to aws lambda with graio step, check lambda function in aws
- get function url and test
- git deployemtn, url is valid only for 400 secs , then destro process will start
- remember, for aws lambda it is cold start, so it will tke some time for URL to load


