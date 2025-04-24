# 📊 Real-Time Sales Stream Processing with AWS

A real-time, industrial-grade solution to process, validate, and transform **sales data** using AWS services like **DynamoDB**, **Kinesis**, **Lambda**, **S3**, **Glue**, and **Athena**.

## 🚀 Project Overview

This project demonstrates a **real-time sales data pipeline** where continuous data from an AWS **DynamoDB** table is streamed, validated, transformed, and stored in **S3**, then queried using **Athena**. This is ideal for use cases like **Sales Projection**, **Live Dashboards**, or **Data Warehousing**.

---

## 🧱 Architecture Components

### 1. **DynamoDB (gadgetOrders Table)**
- A DynamoDB table named `gadgetOrders` was created.
- Change Data Capture (CDC) settings were enabled to stream changes.
- Every new record is streamed for processing.

### 2. **EventBridge Pipes**
- EventBridge Pipe was set up:
  - **Source**: DynamoDB stream.
  - **Destination**: Kinesis Stream.
  - **Partition Key**: `eventID`.

### 3. **Kinesis Data Stream**
- Acts as a middle layer to pass the data from EventBridge to Firehose.
- Source is the EventBridge Pipe.

### 4. **Kinesis Firehose**
- Source: Kinesis Data Stream.
- Destination: S3 Bucket.
- **Data Transformation**: Enabled via AWS Lambda.

### 5. **Lambda Function**
- Attached to Firehose for transformation.
- Validates and transforms each record (record-by-record).
- Only valid, formatted data is stored in the S3 bucket.

### 6. **S3 Bucket**
- Final transformed data is saved here.
- Used for downstream analytics and storage.

### 7. **AWS Glue**
- Glue Crawler was set up to:
  - Crawl the S3 bucket.
  - Create tables in a Glue Catalog Database automatically.

### 8. **Athena**
- Used to run SQL queries directly on the S3 data.
- Quick and cost-effective way to analyze large datasets.

---

## 📈 Flow Summary

```text
DynamoDB ➝ EventBridge Pipe ➝ Kinesis Stream ➝ Kinesis Firehose ➝ Lambda Transform ➝ S3 ➝ Glue ➝ Athena
