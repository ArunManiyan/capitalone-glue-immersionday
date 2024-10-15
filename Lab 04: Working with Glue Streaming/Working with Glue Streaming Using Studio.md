# Working with Glue Streaming

In this lab we will be creating a streaming application using Glue Studio. The AWS account provided to you is already provisioned with a Kensis data stream and catalog entry for the streamed dataset. We can either use catalog table or directly read the data infer schema from the streaming application. In this lab we will try to use the direct read method

## Developing the Glue job using Glue Studio

1. Go to the [AWS Kinesis console](https://console.aws.amazon.com/kinesis/home) and click **Data streams** on the left to open the UI for Kinesis Data Streams. You should see a data stream with name glueworkshop which was created by CloudFormation.
2. Go to the [AWS Glue Console](https://console.aws.amazon.com/glue/home) and click **Data Catalog tables** shortchut on the left-side menu. Click ```json-streaming-table``` to explore the details of the table definition. Click **View properties** under **Actions** button on the upper-right and you will see this table is connect to Kinesis data stream.
3. On the Glue console click on **ETL jobs** from the left panel and click on  **Visual ETL**
4. Rename the job from **Untitled job** to **glueworkshop-lab4-glue-streaming**
5. From the **Add nodes** canvas select **Amazon Kinesis** from the **Sources** tab
6. On the right hand panel, set the **Name** to **StreamingSrc**, select **Stream name** as **glueworkshop** and set **Window size** as 20
  
![streaming-src](https://github.com/user-attachments/assets/f538dfa2-5c64-4b9f-a2e3-0c2eb4b1050e)

7. From the **Add nodes**, go to **Targets** and select **Amazon S3**. Change the **Name** to **TargetDataSet** and **S3 Target Location** to **{BucketName}/output/lab4/**. Leave everything as default value


![streamingop](https://github.com/user-attachments/assets/4e079d1f-8663-4da8-823c-8378b455f7a8)


8. Go to **Job details** and select **AWSGlueServiceRole-glueworkshop** as the **IAM Role**
9. Notice that the **Type** of the job is set to **Spark Streaming**
10. Leave all other setting as default. At this time we have created a job to stream data from a Kensis data stream. Next step we will run the job and stream data into a S3 bucket.

## Running the streaming Job

1. Go to the **AWS Glue Studio Console** click **ETL Jobs** on the left side menu and, under **Your jobs** list, you should be able to see the job you created in the previous section of this lab. Click on the check box near the **glueworkshop-lab4-glue-streaming**, then click on the **Run Job** button

![lab4-job-list](https://github.com/user-attachments/assets/a167dfba-69b0-4889-9f2f-37b407e26367)

2. You will notice a green banner with a Success message at the top of your screen. In this banner, click on **Run Details** to follow the execution of your job.

![lab4-run-details](https://github.com/user-attachments/assets/fae49772-8aeb-4560-adb1-96f5ddd1e21a)

## Send data to Kinesis Data Stream

Once the streaming job is started, we will use a script to publish messages into Kinesis Data Stream. If you recall, during the very initial pre-steps, the boto3 Python library was already installed in your Cloud9 terminal so you could proceed with the following steps in this lab.

3. With the boto3 library already installed for you, open a new Cloud9 terminal and use the following script to add data to the Kinesis data stream.

```
cd ~/environment/glue-workshop
python ~/environment/glue-workshop/code/lab4/PutRecord_Kinesis.py 
```
![infobanner](https://github.com/user-attachments/assets/698f7623-8c01-4a6e-bcd5-746b8a9ed36c)

If you are interested in what the Python script is doing, you can check the script by opening it from ~/environment/glue-workshop/code/lab4/PutRecord_Kinesis.py in Cloud9 IDE.


<img width="1034" alt="lab4-9" src="https://github.com/user-attachments/assets/4123d7ce-0bb6-4825-b9e3-c1f1e94e7158">

## Check streaming job output

Once the Glue ETL Streaming job start running, you can go to the [S3 console](https://us-east-2.console.aws.amazon.com/s3/home?region=us-east-2#)  and to your s3://${BUCKET_NAME}/output/lab4/ folder. You should see a new folder created with a recent timestamp in {YYYMMDDHH24MISS} format. That is the output from the Glue ETL streaming job.

Choose one of the file within the folder which has size > 0 and click the **Object actions** button, then choose **Query with S3 Select** from the menu.

![lab4-s3-select-1](https://github.com/user-attachments/assets/fcd998d7-4ecb-481e-b80e-e90ba3958d02)

Ensure that in **Input setting** the **Format** is select as **Apache parquet** and in **Output settings** the **Format** is select as **CSV** and **CSV delimiter** as **Comma**

4. Click on **Run SQL query** button as it is shown below.

![lab4-s3-select-2](https://github.com/user-attachments/assets/c118ac5c-cf69-41ab-8860-1495e19ac8ab)

## Stop Glue streaming job

5. Once you are done exploring the streaming job details and you have checked the result files, you can stop the streaming job. If you are not there yet, go to the [AWS Glue Console](https://console.aws.amazon.com/glue/home)    Click on **Monitoring**, on the left menu, to see the dashboard with overall job runs summary. Scroll down to the bottom of the dashboard and you should also see your job running there. Click on the radio button for your 
   running job and from there click on the **Actions** button, from the list select **Stop run**.

![lab4-stop-job-run](https://github.com/user-attachments/assets/03ff31d2-d259-4144-99b7-28da3179bbc0)


![info-banner2](https://github.com/user-attachments/assets/e74771ae-84ca-4063-bafd-7384f1e184fa)

This concludes the Lab 4 section of this workshop. We can head back to the main immersion day instructions now. 






