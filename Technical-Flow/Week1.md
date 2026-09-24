# Week 1 - Amazon S3

### Q1. What is Amazon S3 and what does S3 stand for?

**Answer:**

Amazon S3 is a storage service provided by AWS where we can store files such as documents, images, videos, and datasets in the cloud. S3 stands for **Simple Storage Service**.

---

### Q2. What is a region in AWS and why does it matter?

**Answer:**

A region is a physical location where AWS has its data centers. It matters because it affects how quickly data can be accessed and where the data is stored. Choosing the right region can improve performance and meet data storage requirements.

---

### Q3. Why did you create a "data" folder inside the bucket?

**Answer:**

I created a data folder to keep the files organized. As the project grows, there may be many files, so using folders makes it easier to find and manage them.

---

### Q4. What is the difference between a Bucket Name and a File Key?

**Answer:**

The bucket name is the name of the storage container in S3. The file key is the path and filename of a specific file stored inside that bucket.

For example:

```text
s3://ai-intern-nikita/Data/customer_support_tickets.csv

Q5. What is the full S3 path of your file?

Answer:

The full S3 path of my dataset is:

s3://ai-intern-nikita/Data/customer_support_tickets.csv

The cleaned dataset is:

s3://ai-intern-nikita/Data/tickets_clean.csv
```
