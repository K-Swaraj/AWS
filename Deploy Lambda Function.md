# Deploying AWS Lambda function using Git Commands.

### I have used VS Code to push the changes to the Github repo.

## STEP 1 : Create an Lambda Function in AWS.

![Screenshot 2024-08-24 182242](https://github.com/user-attachments/assets/96c002cd-f9d6-4765-bebb-d744e5530f2e)

![Screenshot 2024-08-24 182303](https://github.com/user-attachments/assets/4be9b40d-f7ed-4474-a6f1-7836db89fa2f)

#### This is the default code. You can also make changes according to you but I will go with sample code.

![Screenshot 2024-08-24 182453](https://github.com/user-attachments/assets/faf35f05-d06e-4065-92d8-599d7af63aa8)

## Step 2 : Create folder in local machine and use VS Code.
#### Create a folder in your machine -> Open it in VS -> Open Terminal -> Initialize git -> Clone the link of the github repo.
```'bash
git init
git clone https://github.com/K-Swaraj/AWS.git
cd AWS
git init
```

#### Create a file with Lambda_function.py. NOTE : Name of the file should be same as in AWS Lambda Function. Check above image. Paste the code from AWS Lambda to this created file in VS.

```bash
import json

def lambda_handler(event, context):
    # TODO implement
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }

```
#### Push this file in github and check the changes made.
```bash
git branch
git add .
git commit -m "first commit"
git push origin main
git status
```

#### Now we need to create a simple workflow in a YAMl file so that changes done will be reflected to AWS Lambda function.
#### Create a new folder in VS by name ".github/workflows" and in that folder create a file deploy_lambda_using_git.yaml

#### Explanation : This a workflow which runs on latest ubuntu AMI. First Install a zip and creates a lambda function.
#### IN function name you need to enter the your ARN and your region. ARN is available on AWS Lambda page.
```bash
name: Deploy Lambda Function

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - name: Install zip tool
        uses: montudor/action-zip@v1
      - name: Create Zip file for Lambda function
        run: zip -r code.zip .
      - name: AWS CLI v2
        uses: imehedi/actions-awscli-v2@latest
        with:
          args: "lambda update-function-code \
            --function-name arn:aws:lambda:ap-south-1:898721452106:function:Lambda_test \
            --zip-file fileb://code.zip"
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_DEFAULT_REGION: "ap-south-1"
```


![Screenshot 2024-08-24 184654](https://github.com/user-attachments/assets/bab5744a-1645-4f98-9d68-6558b0aea39b)


## Step 3 : Create an AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY
```bash
1 . GO to AWS IAM User. Click on user -> Security Credentials -> Create Access Keys -> Copy your  "Access key ID" and "Secret Access key" and Paste on github.
2 . On GitHub go to settings -> secrets and variable on LHS -> Action -> New repo secret -> Enter Name and Access Key ID -> Add Secret.
    New repo secret -> Enter Name and Secret Access Key -> Add Secret.
```

#### Save changes in VS Code 
```bash
In lambda_function.py file Change code
" 'body': json.dumps('Hello from Lambda to Git!') "
Save and Push the changes in github

git add .
git commit -m "deployed"
git push origin main
```

### Go to the AWS Lambda Page and check the changes in the code. Now you have successfully deployed lambda function using git and github through VS Code.
