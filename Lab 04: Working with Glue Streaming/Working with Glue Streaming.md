# Develop Glue Streaming Job in Notebook

Before creating a streaming ETL job, you must create a Data Catalog table that specifies the source data stream properties, including the data schema. This table is used as the data source for the streaming ETL job. We will use the Data Catalog table ```json-streaming-table``` in ```glueworkshop_cloudformation``` database that was created earlier by CloudFormation which was deployed while provision the test accounts for you. This table's data source is AWS Kinesis Data Stream and it has the schema definition of the JSON data we will send through the data stream.

**1.** Go to the [AWS Glue Console](https://console.aws.amazon.com/glue/home) and click **Data Catalog tables** shortchut on the left-side menu. Click ```json-streaming-table``` to explore the details of the table definition. Click **View properties** under **Actions** button on the upper-right and you will see this table is connect to Kinesis data stream.

**2.** Go to the [AWS Kinesis console](https://console.aws.amazon.com/kinesis/home) and click **Data streams** on the left to open the UI for Kinesis Data Streams. You should see a data stream with name glueworkshop which was created by CloudFormation.

Amazon Kinesis Data Streams enables you to build custom applications that process or analyze streaming data for specialized needs. You can continuously add various types of data to an Amazon Kinesis data stream from hundreds of thousands of sources. Within seconds, the data will be available for the Glue streaming job to read and process from the stream.

During the streaming processing, we will use a lookup table to convert a country name from the full name to a 2-letter country code. The lookup table data is stored in S3 at ```s3://${BUCKET_NAME}/input/lab4/country_lookup/```.

Next, we will develop the code for the streaming job. Glue streaming is micro-batch based and the streaming job processes incoming data using a Tumbling Window method. All data inside a given window will be processed by a batch function. Inside the Glue Streaming job, the invocation of the tumbling window's function is shown below. The window functions is named batch_function and takes in the micro-batch dataframe, processes it at the window interval (60 seconds in this case).

```
glueContext.forEachBatch(frame=sourceData,
                         batch_function=processBatch,
                         options={"windowSize": "60 seconds", "checkpointLocation": checkpoint_location})
```

