# Project 2: S3 → Lambda → RDS → SNS Automated Pipeline

---

## Introduction

This project implements an event-driven file ingestion pipeline on AWS. Files uploaded from a local machine to an S3 bucket are processed by Lambda, inserted into an RDS database, and notifications are sent via SNS.

It demonstrates how AWS services (S3, Lambda, RDS, SNS, CloudWatch, IAM) can work together to provide a scalable, automated, and reliable pipeline.

---

## Objectives

* Automatically upload files from a local machine to S3.
* Trigger Lambda upon file upload.
* Parse CSV files and insert data into RDS.
* Send notifications via SNS on success/failure.
* Monitor all operations using CloudWatch.
* Apply least-privilege IAM permissions.

---

## AWS Services Used

| Service        | Purpose                                       |
| -------------- | --------------------------------------------- |
| Amazon S3      | Stores uploaded files                         |
| AWS Lambda     | Processes CSV files and inserts data into RDS |
| Amazon RDS     | Stores structured data                        |
| Amazon SNS     | Sends notifications                           |
| AWS CloudWatch | Logs and monitors pipeline events             |
| AWS IAM        | Manages roles and policies for secure access  |

---

## Project Implementation Details

### Step 1: S3 Bucket Setup

* Create an S3 bucket: `my-data-pipeline-bucket-for-automated-file-upload`
* Enable event notifications to trigger Lambda on object creation

**Screenshot:**
![S3 Bucket](Images/s3-bucket.png)

---

### Step 2: Lambda Function

* Python Lambda function processes CSV files, inserts into RDS, and publishes SNS notifications
* Environment variables: `DB_HOST`, `DB_NAME`, `DB_USER`, `DB_PASSWORD`, `SNS_TOPIC_ARN`

**Lambda Handler Example:**

```python
def lambda_handler(event, context):
    bucket = key = "Unknown"
    try:
        bucket = event['Records'][0]['s3']['bucket']['name']
        key = event['Records'][0]['s3']['object']['key']
        logger.info(f"Processing file: {key} from bucket: {bucket}")
        response = s3_client.get_object(Bucket=bucket, Key=key)
        content = response['Body'].read().decode('utf-8').splitlines()
        reader = csv.DictReader(content)
        conn = pymysql.connect(
            host=os.environ['DB_HOST'],
            user=os.environ['DB_USER'],
            password=os.environ['DB_PASSWORD'],
            database=os.environ['DB_NAME'],
            port=int(os.environ.get('DB_PORT', 3306))
        )
        cur = conn.cursor()
        inserted_rows = 0
        for row in reader:
            try:
                sql = """
                INSERT INTO customers (id, name, email)
                VALUES (%s, %s, %s)
                ON DUPLICATE KEY UPDATE
                    name = VALUES(name),
                    email = VALUES(email);
                """
                cur.execute(sql, (int(row['id']), row['name'].strip(), row['email'].strip()))
                inserted_rows += 1
            except Exception as e:
                logger.warning(f"Skipping row {row} due to error: {e}")
        conn.commit()
        cur.close()
        conn.close()
        sns_client.publish(
            TopicArn=sns_topic_arn,
            Subject="Data Insertion Success",
            Message=f"File {key} processed successfully. Inserted/Updated {inserted_rows} rows."
        )
    except Exception as e:
        logger.error(f"Failed to process file {key}: {e}")
        sns_client.publish(
            TopicArn=sns_topic_arn,
            Subject="Data Insertion Failed",
            Message=f"Error processing file {key}: {e}"
        )
        raise e
```

---

### Step 3: RDS Configuration

* RDS (MariaDB) instance for structured data
* Customers table schema:

```sql
CREATE TABLE customers (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100)
);
```

**Screenshot:**
![RDS Schema](Images/rds-schema.png)

---

### Step 4: Local File Upload Script

```python
import boto3

s3 = boto3.client('s3')
bucket_name = 'my-data-pipeline-bucket-for-automated-file-upload'
file_path = 'customers.csv'

s3.upload_file(file_path, bucket_name, 'uploads/customers.csv')
print("Upload successful.")
```

---

### Step 5: IAM & Networking

* Lambda IAM Role Permissions:

  * `AmazonS3ReadOnlyAccess`
  * `AmazonRDSFullAccess`
  * `AmazonSNSFullAccess`
  * `AWSLambdaBasicExecutionRole`
* Ensure Lambda can access RDS securely (VPC if required)

**Screenshot:**
![IAM Policy](Images/iam-policy.png)

---

### Step 6: CloudWatch Monitoring

* Log all Lambda invocations and errors
* Create metric filters for failed inserts
* Configure alarms for high error rates

**Screenshot:**
![CloudWatch Logs](Images/cloudwatch-logs.png)

---

### Step 7: Testing and Validation

| Action                   | Expected Result                |
| ------------------------ | ------------------------------ |
| Upload CSV to S3         | Lambda triggers automatically  |
| CSV contains new records | Records inserted into RDS      |
| Lambda completes         | SNS sends success notification |
| Lambda fails             | SNS sends failure notification |
| Logs                     | CloudWatch captures events     |

---

## Challenges Faced

* IAM permission errors → solved by updating policies
* Edge cases in CSV parsing

---

## Lessons Learned

* Built an event-driven AWS pipeline
* Importance of correct IAM permissions
* Hands-on experience with S3, Lambda, RDS, SNS, CloudWatch
* Improved Python scripting for AWS automation

---

## Conclusion

* Automated ingestion from local machine → S3 → Lambda → RDS → SNS
* Real-time processing and notifications
* Secure, scalable, and reliable architecture

**Screenshots & Outputs:**

* S3 Bucket: `Images/s3-bucket.png`
* IAM Policy: `Images/iam-policy.png`
* CloudWatch Logs: `Images/cloudwatch-logs.png`
* RDS Schema: `Images/rds-schema.png`

---

