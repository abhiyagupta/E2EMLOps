# Hugging Face Deployment
Thsi will deploy trained model in Hugging face spaces via git actions. 

## Features

- Backend two models are trained - one for sports dataset and one for vegfruits dataset
- Front end one UI
- Picks model from s3
- deployed to Hugging face spaces usuing git actions

## Setup

1. create an empty space in Hugging face spaces

2. Get Hugging Face Token having read/write access
- go to your profile in hugging face
- go to access token
- create token with read/write access

3. Add secrets in Hugging face space
- go to empty space created in HF
- go to settings tab  of that space
- scroll to secrets and variables
- add secrets: this will give permission to HF o pull models and json from S3. 
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY

4. git repository secrets, add:
HF_SPACE_NAME  : name of ept space created
HF_TOKEN : the read/write token created
HF_USERNAME : user name of HF spaces

5. the files required for Hugging Face deployment are under gradio_deploy folder
- the readme in thsi folder has to be structed for HF spaces.
- his will have a app.py, requirements.txt and readme.md file

6. Git Actions:
- trigger the hugging face deployemnt yaml
- this will push app.py, requirements.txt and readme.md to hugging face space created
- deployemnt in HF spaces will occur usuing gradio as interface


