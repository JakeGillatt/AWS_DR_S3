# What is DR?

DR stands for Disaster Recovery plan. A disaster recovery plan is a documented strategy that outlines the procedures and systems necessary to recover and 
restore critical technology infrastructure and operations following a cyber-attack, natural disaster, or other disruptive event that could destroy data and systems.

The goal of a disaster recovery plan is to minimize the impact of the disaster on the organization's operations, and ensure the continuity of business operations 
and services in the event of an unforeseen event. The plan typically includes detailed procedures for data backup and recovery, alternative communication systems, 
and procedures for restoring critical systems and applications.

#
# What is S3?

Amazon Simple Storage Service (S3) is a highly scalable and durable object storage service offered by Amazon Web Services (AWS). It is designed to store and retrieve 
any amount of data, at any time, from anywhere on the web.

S3 is used by businesses and individuals to store and manage a wide variety of data types including:

- Media files
- Backups
- Application data
- Website content

It is commonly used for data archiving, data backup and recovery, content distribution, and cloud-native application data storage.

- S3 uses "Buckets" (folders) and "Objects" (files)

#
# What is AWSCLI?
AWS CLI (Command Line Interface) is a unified tool that provides a command-line interface for managing and interacting with various AWS services. It enables 
developers and system administrators to automate tasks and interact with AWS services directly from their terminal or command-line interface.

AWS CLI is a powerful tool that enables developers and system administrators to automate tasks and manage AWS resources more efficiently, helping to reduce 
the time and effort required to manage complex cloud infrastructure.

#
# What is SDK?

SDK stands for Software Development Kit, which is a collection of tools and resources that developers use to create software applications for a specific platform or programming language.

SDKs provide developers with the necessary tools and resources to build applications that leverage the features and capabilities of a particular platform or programming language. They typically include APIs (Application Programming Interfaces), sample code, and documentation that help developers understand how to use the platform and build applications that integrate with it.

#
# Configuring S3 access and installing AWSCLI

After launching a Ubuntu instance with SSH:
1. SSH into the instance and run the following commands
```
sudo apt update -y
sudo apt upgrade -y
sudo apt install python -y
sudo apt install python3-pip
alias python=python3 # This assigns a temporary variable
sudo python3 -m pip install AWSCLI # This installs the CLI
```
2. Now run the command `aws configure`
3. Enter your Access key, Secret key, Region (eu-west-1) and Output format (json)
4. Use the command `aws s3 ls` to list everything on the s3 and test the connection. Where:
- aws - the cloud name
- s3 - the service name
- ls - the command we wish to use

#
# Sending and receiving files from S3 to local

To create a bucket on the S3: `aws s3 mb s3://jake-tech221` - this creates a bucket called jake-tech221
To upload a file to the bucket: `aws s3 cp 'example-file' s3://jaketech221/`
To download the file from the bucket: `aws s3 cp s3://jake-tech221/'example-file' 'local dir'/`
- `aws s3 sync` can be used to sync all files in the bucket
- `aws s3 rb 'bucket'` can be used to delete a bucket

#
# Connecting to S3 Diagram:
![S3-Diagram](https://user-images.githubusercontent.com/129315605/234900533-db5a0fae-74d1-4806-92d1-cb794b2e0d76.png)

#
# Using a python script with boto3 to carry out S3 related tasks:

1. Install boto3 with `pip3 install boto3`
2. Create a new python file with `sudo nano 'filename'.py` 
3. Enter the code you require. Here is some examples:
- Here is a script that creates a new bucket on S3:
```
import boto3

AWS_REGION = "eu-west-1"

client = boto3.client("s3", region_name=AWS_REGION)

bucket_name = "jake-tech221"

location = {'LocationConstraint': AWS_REGION}

response = client.create_bucket(Bucket=bucket_name, CreateBucketConfiguration=location)

print("Amazon S3 bucket has been created")
```
- Here is a script that uploads a file to the S3 bucket:
```
import boto3

# Set up the S3 client
s3 = boto3.client('s3')

# Set up the local file path and S3 bucket/key
local_file_path = 'test/test/test.txt'
s3_bucket = 'jake-tech221'
s3_key = 'test.txt'

# Upload the file to S3
s3.upload_file(local_file_path, s3_bucket, s3_key)

print(f"{local_file_path} uploaded to s3://{s3_bucket}/{s3_key}")
```
- Here is a script that downloads a file from the S3 bucket:
```
import boto3

# Set up the S3 client
s3 = boto3.client('s3')

# Set up the S3 bucket and file key
s3_bucket = 'jake-tech221'
s3_file_key = 'test.txt'

# Set up the local file path to download to
local_file_path = 'test.txt'

# Download the file from S3 to the local directory
s3.download_file(s3_bucket, s3_file_key, local_file_path)

print(f"File s3://{s3_bucket}/{s3_file_key} downloaded to {local_file_path}")
```

