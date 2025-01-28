# Portfolio Deployment

This repository contains a GitHub Actions workflow that automates the deployment of a static portfolio site to an AWS S3 bucket. The deployment process is triggered whenever changes are pushed to the `main` branch.

## Workflow Overview

### Trigger
The workflow is triggered on a push to the `main` branch.

```yaml
on:
  push:
    branches:
      - main
```
## Jobs
The workflow contains a single job named build-and-deploy that runs on the latest version of Ubuntu. Below are the steps involved in this job:

Checkout Code: The first step uses the actions/checkout action to clone the repository and get the code.

```yaml
- name: Checkout
  uses: actions/checkout@v1
```
## Configure AWS Credentials: The next step configures the AWS credentials needed for interacting with AWS services (in this case, S3).
It uses the aws-actions/configure-aws-credentials action and fetches AWS credentials from the repository's secrets (AWS access key and secret access key).

```yaml
- name: Configure AWS Credentials
  uses: aws-actions/configure-aws-credentials@v1
  with:
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: us-east-1
```
## Deploy to S3: The final step uses the aws s3 sync command to sync the contents of the repository (local files) to the target S3 bucket (tws-junoon). 
The --delete option ensures that any files in the bucket that are not present locally will be deleted.

```yaml
- name: Deploy static site to S3 bucket
  run: aws s3 sync . s3://tws-junoon --delete
```

Got it! Below is the README.md content with the YAML code properly formatted and indented, ready for you to copy and paste directly into your README.md file.

markdown
Copy
# Portfolio Deployment

This repository contains a GitHub Actions workflow that automates the deployment of a static portfolio site to an AWS S3 bucket. The deployment process is triggered whenever changes are pushed to the `main` branch.

## Workflow Overview

### Trigger
The workflow is triggered on a push to the `main` branch.

```yaml
on:
  push:
    branches:
      - main
```
Jobs
The workflow contains a single job named build-and-deploy that runs on the latest version of Ubuntu. Below are the steps involved in this job:

Checkout Code: The first step uses the actions/checkout action to clone the repository and get the code.

```yaml
- name: Checkout
  uses: actions/checkout@v1
```
Configure AWS Credentials: The next step configures the AWS credentials needed for interacting with AWS services (in this case, S3). It uses the aws-actions/configure-aws-credentials action and fetches AWS credentials from the repository's secrets (AWS access key and secret access key).

```yaml
- name: Configure AWS Credentials
  uses: aws-actions/configure-aws-credentials@v1
  with:
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: us-east-1
```
AWS Access Key and Secret Access Key are stored securely in the repository's secrets to ensure that they are not exposed.
AWS Region specifies the region where the S3 bucket is located (in this case, us-east-1).
Deploy to S3: The final step uses the aws s3 sync command to sync the contents of the repository (local files) to the target S3 bucket (tws-junoon). The --delete option ensures that any files in the bucket that are not present locally will be deleted.

```yaml
- name: Deploy static site to S3 bucket
  run: aws s3 sync . s3://tws-junoon --delete
```
## Sync Command: This command uploads all files in the repository to the specified S3 bucket and ensures the bucket's contents are synchronized with the local files.
S3 Bucket Permissions Policy
In order to allow public access to the files in your S3 bucket, you need to set up an appropriate bucket policy. The following policy grants public read access to all objects within the bucket:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
```
