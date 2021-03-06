![awsnw](https://user-images.githubusercontent.com/72220952/100804872-7ec1da80-3425-11eb-8e5a-708862ed0957.jpg)

The below section includes my works done as part of exploring the diverse opportunities of using various AWS services. Amazon Web Services provide reliable and scalable cloud computing solutions. It is one of the world's most adopted cloud platform for various solutions. In this section, I have attempted to explore few of the Data Engineering related services provided by amazon web service.

Let's explore the activities done as part of this learning experience.

# 1. Deliver streaming data to S3 bucket using AWS Kinesis Firehose

This activity demonstrates how to deliver the streaming data set to an S3 bucket using amazon's Kinesis Data Firehose.

To start with this hands-on, the prerequisites include:
* Active AWS account
* Python

the various steps involved in this Hands-on includes:

* Step1: Installation of AWS CLI

```
pip install awscli
```
* Step2: Installation of Boto3

```
pip install boto3
```

* Step3: Creation of Delivery Stream in Kinesis Firehose

You have to mention the below details for creating the delivery stream.
Delivery Stream Name
1. Source: Choose Direct PUT or other sources
2. Data transformation: Disabled (Default)
3. Record format conversion: Disabled (Default)
4. Destination: S3
5. S3 Destination: You have to choose the option to create a new bucket to store the data
6. S3 Buffer Conditions: (Default settings)
7. S3 compression and encryption: (Default settings)
8. Permissions: Select the option to create a new IAM role for the Data firehose
9. Review and create the DeliveryStream.

* Step4: Python Script for data generation

Now we need to develop a python script to generate some streaming data to ingest into the Delivery Stream created above.

This script is given as streaming.py, which generates the streaming data.

In order to connect with the Kinesis Data Firehose using Boto3, we need to use the below commands in the script.

```
Kinesisstream=boto3.client('firehose')
```

* Step5: Stream the Data to KinesisFirehose using PutRecordBatch() API

 To ingest data, we use the Put_Record_Batch() API as given below.

```
result = Kinesisstream.put_record_batch(DeliveryStreamName="SampleNumbers",
                                                   Records=records)
```
Once we run this script, it ingests the generated data to the delivery stream created.


* Step6: Monitor the Firehose dashboard to view data movement.

* Step7: Validate S3 data

Go to the S3 bucket and under the bucket created during the DeliveryStream configuration, we can see the data loaded by the Kinesis Firehose.

Now you have successfully delivered your streaming data set to the S3 storage using the Kinesis Data Firehose service!

# 2. AWS Glue

# Serverless data pipelines using Amazon Glue to move data between AWS S3 buckets 

This activity can consist of the below steps.

* Step 1: Creation of a source S3 bucket
* Step 2: Uploading desired data to the S3 bucket created
* Step 3: Creation of the target S3 bucket to copy the data
* Step 4: Creation of Glue crawlers
* Step 5: Creation of the ETL jobs for the data movement between S3 buckets

### Step 1: 
An S3 bucket and a prefix are created to place the data file.
![image](https://user-images.githubusercontent.com/72220952/94967920-ed52ec80-04f7-11eb-825b-6e9064a0bc67.png)

### Step 2: 
Data downloaded from the Kaggle has been uploaded into the S3 bucket created in previous step. The data set known as Iris.csv was used in this example
### Step 3: 
A target S3 bucket is created, which act as the target bucket to move the data placed in the source S3 bucket
### Step 4: 
AWS Glue crawler is created to create the meta data catalog about the source S3 buckets. 
In order to create the crawler below options were selected in the console.

	* Crawler source type -Data Stores
	* Choose data store -S3
	* Crawl data in - path of the source s3 prefix
	* Choose an IAM role - select an IAM role with necessary permisions on the Glue. A new role with below policies were 
	  created to perform this hands-on.
	
![image](https://user-images.githubusercontent.com/72220952/94969010-df9e6680-04f9-11eb-8936-ae408371b268.png)
	
	* Frequency - run on Demand
	* Crawler output - Create a new database where the crawler stores the metadata information of the sources.
	

Once the crawler is ready, we can run the same to generate the meta data details. On successful run, the specified database will be created and we can verify that a metadata table is created in the databasewith the schema details.

![image](https://user-images.githubusercontent.com/72220952/94969681-15901a80-04fb-11eb-8346-79739ed444be.png)

 
### Step 5: 
A Glue ETL job is created to move data from Source S3 bucket to target bucket. Below options were selected to create the ETL job.

	* Type -Spark
	* Glue version -default
	* This job runs - A proposed script generated by AWS Glue
	* Specify script file name and s3 path to store the script

Select the datasource which we have created in the previous steps. and transform type is selected as ' Change schema'. 
In the choose data target page, below options were opted.
	
	* Data Store -Amazon S3
	* Format - CSV
	* Target path - select the created target bucket in the S3

Next page shows the details of source and target column mappings.

![image](https://user-images.githubusercontent.com/72220952/95356713-dda02300-08be-11eb-8635-bc355cfba29b.png)


The default script generated by AWS is includede in datapipeline.py.



# 3. Loading Kaggle dataset to AWS S3 using Boto3


This activity intends to download the dataset from the Kaggle site using the available Kaggle API and load the data to an AWS S3 bucket with the help of python Boto3.

The entire activity of fetching data from Kaggle to S3 has been broken down as different steps. Which includes :

* Step 1: Installation of Kaggle CLI
* Step 2: Installation of AWS CLI
* Step 3: Installation of Boto3
* Step 4: AWS S3 bucket creation using Python Boto3
* Step 5: Kaggle Dataset download
* Step 6: File uploads to AWS S3 using Boto3

### Step 1: Installation of Kaggle CLI

```
pip install kaggle

```
### Step 2: Installation of AWS CLI

```
pip install awscli

```
### Step 3: Installation of Boto3

```
pip install boto3

```
### Step 4: AWS S3 bucket creation using Python Boto3

```
import boto3
s3resource=boto3.client('s3','us-east-1')
s3resource.create_bucket(Bucket='filetransfers3tords')
```
### Step 5: Kaggle Dataset download

```
kaggle datasets list
kaggle datasets download dataset name -p destination -unzip
```
### Step 6: File uploads to AWS S3 using Boto3

```
s3resource.upload_file(KaggleDatasetname,bucket_name,S3storedkaggledatasetname)

```

Note: For Detailed explanation of above activity, visit my blog at:  https://medium.com/@antonysruthy11/loading-kaggle-dataset-to-aws-s3-using-boto3-50af3e015fb2
