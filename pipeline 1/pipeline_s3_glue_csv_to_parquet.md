# 🚀 Pipeline 1: S3 → Glue (CSV → Parquet)

---

## 🎯 Goal

As soon as a CSV file is uploaded into an S3 bucket:

- [ ] AWS Glue reads CSV data  
- [ ] Glue converts it into Parquet format  
- [ ] Parquet file is stored in refined S3 bucket  
- [ ] Job logs execution in CloudWatch  

✅ End-to-end batch data transformation pipeline  

---

## 📦 Required AWS Resources

| Sr | Service | Name |
|----|----------|------|
| 1 | S3 Raw Bucket | kshitij-raw-bucket |
| 2 | S3 Refined Bucket | kshitij-refined-bucket |
| 3 | Glue Job | csv-to-parquet-job |
| 4 | IAM Role | data-engineer-role |

---

## 🔐 IAM Role Configuration (MOST IMPORTANT)

Reuse existing role:

```
data-engineer-role
```

---

### ✅ Step 1: Attached Managed Policies

- [ ] AmazonS3FullAccess  
- [ ] AWSGlueServiceRole  
- [ ] CloudWatchLogsFullAccess  

---

### ✅ Step 2: Trust Policy (Glue Assume Role)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "glue.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

---

## 📂 S3 Bucket Setup

### ✅ Step 1: Create Raw Bucket

```
kshitij-raw-bucket
```

Upload:

```
employee_data.csv
```

---

### ✅ Step 2: Create Refined Bucket

```
kshitij-refined-bucket
```

(No special configuration required)

---

## 🔧 Glue Job Configuration

---

### ✅ Step 1: Create Glue Job

| Setting      | Value              |
| ------------ | ------------------ |
| Job Name     | csv-to-parquet-job |
| IAM Role     | data-engineer-role |
| Glue Version | 4.0                |
| Worker Type  | G.1X               |
| Workers      | 2                  |

---

### ✅ Step 2: Glue Script (Script Editor)

```python
import sys
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job

# Initialize
sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
job = Job(glueContext)
job.init("csv-to-parquet-job", {})

# Read CSV from raw bucket
df = spark.read \
    .option("header", "true") \
    .option("inferSchema", "true") \
    .csv("s3://kshitij-raw-bucket/employee_data.csv")

# Write as Parquet to refined bucket
df.write \
    .mode("overwrite") \
    .parquet("s3://kshitij-refined-bucket/employee_parquet/")

job.commit()
```

---

## 📊 Expected Output

```
s3://kshitij-refined-bucket/employee_parquet/

    part-00000-xxxx.snappy.parquet
    _SUCCESS
```

---

## 📈 CloudWatch Logs Verification

Check:

```
/aws-glue/jobs/output
```

Expected:

- [ ] Job started successfully
- [ ] CSV read successfully
- [ ] Parquet written successfully
- [ ] Job completed

---

## 🧠 Learning Outcome

After completing this pipeline, you understand:

- S3 as raw layer
- Glue as transformation engine
- Parquet as optimized columnar format
- CloudWatch for monitoring

---

## 🔥 Upgrade Options (Next Level)

- [ ] Add Partitioning (department-wise)
- [ ] Add Glue Data Catalog table
- [ ] Enable Job Bookmark
- [ ] Parameterize Glue Job
- [ ] Trigger using EventBridge
- [ ] Add Redshift load stage

---

## ✅ Status Tracker

| Task             | Status |
| ---------------- | ------ |
| Buckets Created  | ⬜      |
| CSV Uploaded     | ⬜      |
| Glue Job Created | ⬜      |
| Script Executed  | ⬜      |
| Parquet Verified | ⬜      |

---

💡 Save this file and reuse it for practice anytime.
