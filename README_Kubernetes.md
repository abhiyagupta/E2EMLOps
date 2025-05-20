NOTES change: 
- when running in local first create a folder
```
mkdir dev
cd dev
git clone E2EMLOps
```
-  when cloning argo cd repo, again go to dev path
```
cd dev
git clone E2EMLOps_argocd_kserve
```
- line 24 in sports-classifier.yml (K*-->eks cluster)
- same for veg fruits classfier
- test_kserve_sports_2.py - change url. url shd be copied from load balancers dns
- after adding poliy- go to model server cm in argo cd and change dns 
- when opening a port - go to new terminal
- in new terminal add """ export PATH=$HOME/bin:$PATH """
- ARGO cd: check below. clone argo cd repo in same workspace first. then follow steps. 
- chnage the argo cd at relevant places. 
- after cloning argo cd- add s3secrets.yaml 

```
export PATH=$HOME/bin:$PATH
```

- if policy is not there when running add policy for cluster- manully create policy

commands to create aws policy 
```
curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.11.0/docs/install/iam_policy.json

aws iam create-policy --policy-name AWSLoadBalancerControllerIAMPolicy --policy-document file://iam-policy.json
```


1. step 1 : create three docker images:
```
cd K8SDeploy/src/web-server 
docker build -t web-server -f Dockerfile.web-server . 
cd K8SDeploy/src/ui-server
docker build -t ui-server -f Dockerfile.ui-server .
cd K8SDeploy/src/model-server 
docker build -t model-onnx-server -f Dockerfile.model-onnx-server . 
```

2. Step 2:  aws console- create three ecr repo all private
- a18/ui-server 
- a18/web-server
- model-onnx-server

3. push three images created to these repo's .
- click on a18/ui-server in aws- view push commands
- configure aws 
```
sudo apt install update
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install
aws configure
```
- exceute command 1 , 3,4 
- note remove a18 only from tag before docker in step 3 as our bild name is different. 
```
cd K8SDeploy/src/model-server
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 390844750487.dkr.ecr.ap-south-1.amazonaws.com
docker tag model-onnx-server:latest 390844750487.dkr.ecr.ap-south-1.amazonaws.com/model-onnx-server:latest
docker push 390844750487.dkr.ecr.ap-south-1.amazonaws.com/model-onnx-server:latest
```
```
cd K8SDeploy/src/ui-server
docker tag ui-server:latest 390844750487.dkr.ecr.ap-south-1.amazonaws.com/a18/ui-server:latest
docker push 390844750487.dkr.ecr.ap-south-1.amazonaws.com/a18/ui-server:latest
```

``` 
cd K8SDeploy/src/web-server 
docker tag web-server:latest 390844750487.dkr.ecr.ap-south-1.amazonaws.com/a18/web-server:latest
docker push 390844750487.dkr.ecr.ap-south-1.amazonaws.com/a18/web-server:latest
```
4. from E2EMLOps read me Deployment 01 , execute commands starting from 
```
cd E2EMLOps

```
- continue from read me 
- /K8SDeploy/eks-cluster-config/eks-cluster.yaml: comment out line no 20 publickKeyPath to run in local
- this is reuired to run in git actions , but not in local
- also line 40

```
ssh-keygen -t rsa -b 4096
```
- just enter to save key


- once the creation starts, go to aws console cloud formation to see. 
- cluster creation takes about 10 mins - four clusters in cloud formation
- once cluster is created push image to aws -to save for ref.
- after pushing image , we can check in ami in aws -if usuing ec2 and not gitpod

```
aws ec2 create-image \
  --instance-id i-xxxxxxxxxxxxxxxxx \
  --name "MyServerImage" \
  --description "AMI of my EC2 instance" \
  --no-reboot
``` 

- metrics server
- you will see patched after running above command
- KNative 
- ISTIO
- check everyhting is running 
- all 3 services. wait 
```
kubectl get pods 

kubectl get all -n cert-manager
```
- KNative Serving

- S3 Access creation

- run dashboard ALB commands
- ALB cmmand run -but change account name to point to your aws
- before running ALB policy command , edit the changes change 1: change classifer name
- now EKS chart commands
- test url correct command --
- kubectl apply -f sports-classifier.yml 
- after running above check pods 
```
kubectl get pods
```
- at this step change url from load balancer- change step 2 (test_kserve_sports_2.py)
- url should point to aws --> loadbalancer (under ec2) --> dns 

- Argo CD deployment: clone argo cd repo to this space 
``` 
cd workspace
git clone https://github.com/abhiyagupta/E2MLOps_argo_cd_kserve.git
``` 
    

