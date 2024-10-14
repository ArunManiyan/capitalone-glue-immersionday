# Working with GLUE ETL

In this lab we will develop, deploy and run spark ETL using the Glue's studio enviornment.

## Developing ETL code

1. Go to the [AWS Glue Console](https://console.aws.amazon.com/glue/home) and click **ETL Jobs** from the left hand panel
2. Click on **Script editor**

----------------------------------------------------------------------------------------------------------------
 
   ![visualETL](https://github.com/user-attachments/assets/96cdfa92-a273-473d-ab88-bb0de66e8e33)

----------------------------------------------------------------------------------------------------------------
 
3. Click on **Create script**
4. Replace the default script with below script

```
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.dynamicframe import DynamicFrame
from awsglue.job import Job

# Important further required libraries

from pyspark.sql.functions import udf, col
from pyspark.sql.types import IntegerType, StringType
from pyspark import SparkContext
from pyspark.sql import SQLContext
from datetime import datetime

# Starting Spark/Glue Context

args = getResolvedOptions(sys.argv, ['JOB_NAME','s3_bucket'])
sc = SparkContext.getOrCreate()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
job = Job(glueContext)

input_bucket = args['s3_bucket']

from pycountry_convert import (
    convert_country_alpha2_to_country_name,
    convert_country_alpha2_to_continent,
    convert_country_name_to_country_alpha2,
    convert_country_alpha3_to_country_alpha2,
)


# Defining the function code
def get_country_code2(country_name):
    country_code2 = 'US'
    try:
        country_code2 = convert_country_name_to_country_alpha2(country_name)
    except KeyError:
        country_code2 = ''
    return country_code2

# leveraging the Country Code UDF

udf_get_country_code2 = udf(lambda z: get_country_code2(z), StringType())


#Reading the dataset into a DataFrame
s3_bucket = "s3://"+input_bucket +"/"                              
job_time_string = datetime.now().strftime("%Y%m%d%H%M%S")

df = spark.read.load(s3_bucket + "input/lab2/sample.csv", 
                     format="csv", 
                     sep=",", 
                     inferSchema="true", 
                     header="true")

# Performing a transformation that adds a new Country Code column to the dataframe based on the Country Code UDF output

new_df = df.withColumn('country_code_2', udf_get_country_code2(col("country")))


# Sinking the data into another S3 bucket path

new_df.write.csv(s3_bucket + "/output/lab2/sales_country/" + job_time_string + "/")

```
5. Rename the job name from **Untitled job** to
   ```
   lab2-console-glue-job
   ```
 ----------------------------------------------------------------------------------------------------------------
 
![job-rename](https://github.com/user-attachments/assets/e855b533-be7f-4d86-ab30-768347ca1182)

----------------------------------------------------------------------------------------------------------------
 
6. Click on **Job details** and make following changes
   
    **a.** Select **AWSGlueServiceRole-glueworkshop** as the **IAM Role**
   
    **b.** Change the **Job timeout (minutes)** to 10

----------------------------------------------------------------------------------------------------------------
 
![job-prop](https://github.com/user-attachments/assets/1d417024-f5b7-45b6-87e3-d2844f390fab)

----------------------------------------------------------------------------------------------------------------
 
  **c.** Expand the **Adavanced properties**, scroll down to **Job parameters** section and add a paramenter with **Key**

   **--s3_bucket** and enter the bucketname from your account in **Value-optional** field

  **d.** Add another parameter **--extra-py-files** and **s3://{BUCKET_NAME}}/library/pycountry_convert.zip**. Ensure that BUCKET_NAME is updated with your bucket name before running the job

----------------------------------------------------------------------------------------------------------------
 
![job-parameters](https://github.com/user-attachments/assets/f4ba637c-8b04-4f1d-8b80-784b7c4f48d1)

----------------------------------------------------------------------------------------------------------------
 
7. Click on **Save** to save the job


## Running the job

1. Click on the **Run** and click on **Run details** from the top green banner

----------------------------------------------------------------------------------------------------------------
 
   ![run-details](https://github.com/user-attachments/assets/06e0ac41-19ae-41aa-b811-943866312f3e)

----------------------------------------------------------------------------------------------------------------
 
2. Wait until the **Run Status** changes from **Running** to **Succeeded**

3. Navigate to the S3 output folder and ensure that the outfile was created.
  
    

   
