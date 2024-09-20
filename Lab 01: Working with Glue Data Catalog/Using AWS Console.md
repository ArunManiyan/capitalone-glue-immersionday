# Using AWS Console

We will create a Glue database, a crawler with 2 data sources to crawl CSV and JSON folders using AWS Console - be sure to replace ${BUCKET_NAME} with your bucket name.

Before we use the Glue crawler to scan the files, we will first explore the file contents inside Cloud9. From the Cloud9 terminal, use the following commands to view the data we'll use for this lab. You can see the data format and later compare this to the results from the Glue crawler.

```
head ~/environment/glue-workshop/data/lab1/csv/sample.csv

```

Go to the AWS Glue console , click **Databases** on the left under **Data Catalog** section. You should see a database with name glueworkshop-cloudformation. This was created by the CloudFormation template we launched during workshop setup and contains two pre-defined tables that we will use later in Glue streaming lab.

## Create Database

**1.** Create another database with name ``` console_glueworkshop ``` by clicking **Add Database**.

**2.** Clicking **Create Database**.

## Next Step - Create Glue Crawler

We will create one crawlers to crawl 2 data sources each with CSV and JSON folders - be sure to replace **${BUCKET_NAME}** with your bucket name.

### Create Crawler

  **1.** Click **Crawlers** on the left under **Data Catalog** Section.
  
  **2.** Click **Create Crawler.**
  
  **3.** On **Set crawler properties** page, provide a name for the new Crawler such as 
  ``` 
  console-lab1
```
  , click Next.
  
  **4.** On **Choose data sources and classifiers** page, select **Not Yet** under Data source configuration 
  
  **5.** Click on **Add a data store**, In S3 path browse to ``` s3://${BUCKET_NAME}/input/lab1/csv/``` .
  Make sure you pick the csv folder rather than the file inside the folder, and then click **Next** and keep rest of the option default
