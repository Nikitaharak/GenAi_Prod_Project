# Week 2 - Amazon SageMaker Studio

## Q1. What is Amazon SageMaker Studio and how is it different from writing Python code on your laptop?

**Answer:**

Amazon SageMaker Studio is a cloud-based development environment where I can write, run, and manage Python code directly on AWS. Unlike my laptop, it already has data science tools installed and is directly connected to AWS services like S3, making it easier to work with cloud data.

---

## Q2. What is an IAM Role and why did SageMaker need this permission to access S3?

**Answer:**

An IAM Role is a set of permissions that allows an AWS service to access other AWS resources securely. SageMaker needed the `AmazonS3FullAccess` permission so it could read files from my S3 bucket and save files back to S3.

---

## Q3. What do `pandas`, `boto3`, and `io` do?

**Answer:**

- **pandas:** Used to load, analyze, and work with tabular data.
- **boto3:** Used to connect Python code with AWS services such as S3.
- **io:** Used to read file data from memory as a file-like object.

---

## Q4. What is a Jupyter Notebook kernel and why did you choose "Python 3 (Data Science)"?

**Answer:**

A kernel is the engine that runs the code inside a Jupyter Notebook. I selected the **Python 3 (Data Science)** kernel because it already includes common data science libraries like Pandas, NumPy, and Matplotlib, so I did not need to install them manually.

---

## Q5. Explain `s3.get_object(Bucket=BUCKET, Key=KEY)`.

**Answer:**

`get_object()` is a function used to retrieve a file from S3. `Bucket` specifies which S3 bucket contains the file, and `Key` specifies the exact path and filename of the file inside that bucket.

---

# Scenario-Based Questions

## Q6. SageMaker shows "Kernel is dead". What does it mean and how do you fix it?

**Answer:**

A "Kernel is dead" error means the Python environment running the notebook has stopped working. I would restart the kernel, refresh the notebook, and run the cells again. If the problem continues, I would restart the SageMaker notebook instance or Studio session.

---

## Q7. You get an "AccessDenied" error. Bucket and key are correct. What is the cause?

**Answer:**

The most likely cause is that SageMaker does not have permission to access the S3 bucket. I would check the IAM Role attached to SageMaker and make sure it has the required S3 permissions, such as `AmazonS3FullAccess`.

---

## Q8. Why use SageMaker instead of Google Colab?

**Answer:**

Google Colab is useful for learning, but SageMaker is better for this project because it is already integrated with AWS services. It can directly access S3 data, train machine learning models, and deploy them within the AWS environment without additional configuration.

---

## Q9. `df.head()` works but `df.shape` shows only 500 rows instead of 8,469. What could have happened?

**Answer:**

The file may not have loaded completely, the wrong file may have been selected, or some filtering step may have reduced the data. I would first check the file path, verify the file in S3, and inspect the DataFrame to understand why only 500 rows were loaded.

---

## Q10. Why is `df` showing "NameError" after reopening SageMaker Studio?

**Answer:**

Variables only exist in memory while the notebook session is running. When SageMaker is closed or the kernel restarts, all variables are cleared. To fix it, I need to rerun the cells that load and process the data so that `df` is created again.
