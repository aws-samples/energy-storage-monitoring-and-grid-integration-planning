This will be the Architecture:

 ![Fig 1](../image/architecture.png)

> [!IMPORTANT]
> To do the following setup, do everything in the same region

> [!IMPORTANT]
> You will find all the required code in the attached folder with the name **energy-storage**

<!-- > [!IMPORTANT]
> Must replace S3 bucket name, and to avoid creating many buckect it is recommended to provide same s3 buckect name in all three definations of the Step Function and lambda functions.  -->


## Prerequisite Setup Steps for 2a in above Architecture Overview:

**1.** Create S3 bucket to store data for testing purpose.

Step-1 Navigate to S3 console.

Step-2. Click on **Create bucket**

  ![Fig 5](../image/TestS3Bucket/1.png)

Step-3. Provide name for bucket.  

  ![Fig 5](../image/TestS3Bucket/2.png) 

Step-4. Scroll down and click on **Create bucket**

  ![Fig 5](../image/TestS3Bucket/3.png) 

**2.** Now create four folders inside this with name **betteries_info**, **batteries_measurements**, **renewable-plants-info** and **feeders-info**

  - Now click on the bucket to open it. click on **create folder** button and give the name as **betteries_info** and click on **create folder**.
  
  ![Fig 5](../image/TestS3Bucket/4.png)

  ![Fig 6](../image/TestS3Bucket/5.png)

  - Repeat the same steps for **batteries_measurements**, **enewable-plants-info** and **feeders- info**.

  ![Fig 7](../image/TestS3Bucket/6.png)

  - After creating folder, upload the given ***.csv*** file to the respective folder. To get the csv file go to **energy-storage** folder. 

  - Go to **S3 data** folder to find **.csv** file for all four folders with the same name as folder name. Download all four csv file.

**3.** Upload files in respective folder

  - Open the Bucket and click on folder to open it, then click on **Upload button**

  ![Fig 8](../image/TestS3Bucket/7.png)

  - Click on **Add Files**

    ![Fig 9](../image/TestS3Bucket/11.png)

  - Choose the downloaded file. 
  
  ![Fig 9](../image/TestS3Bucket/8.png)
  
  - Click on **Upload** at bottom

  ![Fig 10](../image/TestS3Bucket/9.png)

**4.** Follow the same steps for all four folder and choose the respective files which is same as folder name.

### Step 2. Create AWS Step Function

 This state machine should have steps that include invoking the Athena query and triggering Lambda functions.

**To set up your step function**

 Here are the general steps to create state machine in Step Functions :

**1.** Navigate to Step Functions

 - Go to the AWS Management Console and navigate to the **StepFunctions** service.

**2.** Create a State Machine
 
- Click the **Create state machine** button.

![Fig 11](../image/StepFunction/1.png)

- Choose **Blank** template and click on **Select**.

![Fig 1212](../image/StepFunction/11.png)

- Click on edit icon from top left and change the name of step function to **es-storagetwin-stepf**.

![Fig 12121](../image/StepFunction/12.png)

![Fig 1222](../image/StepFunction/13.png)

- Now click on **Code** section.

- To get the code please refer folder energy-storage. Inside energy-storage go to folder Step Function Code and copy the code inside **es-storagetwin-stepf** and paste it. Then click on **Create**.

> [!NOTE] 
> Change the **[AccountNo]** with your AWS account number and also change the **[region]** where you want to deploy these resources. Must do this changes for upcoming step function in 2b and 2c as well.

> [!NOTE]
> Also change the **[BucketName]**.

**Example**

Refer the below example on how to change the **region**, **AccountNumber** and **BucketName** name. As you can see in line number 12 and 22 in below figure. In this example i have provided dummy value.

![Fig 82](../image/Examples/1.png)

![Fig 83](../image/Examples/2.png)

> [!NOTE]
> It is recommended use same BucketName in all three step function to avoid multiple bucket creation.

![Fig 1311](../image/StepFunction/14.png)

- Click on **Create**.

- Then click on **confirm**.

![Fig 1311](../image/StepFunction/15.png)

**3.** Your state machine will get created 

  - Click on to the **IAM role arn** as highlighted in below image.

  ![Fig 16](../image/StepFunction/6.png)
  
  - Click on **add permission** and choose **attach policies**.

  ![Fig 17](../image/StepFunction/7.png)

  - Search for following one by one and select all this:
    **AmazonSQSFullAccess**, **AmazonS3FullAccess**, **AmazonAthenaFullAccess**, **AmazonForecastFullAccess** and now click on **add permission**, this will give required permission to your step function.

  ![Fig 18](../image/StepFunction/8.png)

  ![Fig 19](../image/StepFunction/9.png)

### Step 3. Create table inside the AWS Glue Data Catalog and map it to actual data in Amazon S3.
By creating a table in the Glue Data Catalog, you define the schema of your data. This includes specifying column names, data types, and potentially partition keys. This schema information is crucial for properly interpreting and querying your data using tools like Athena.

**Prerequisite for Glue is to create IAM Role.**

  - Navigate for IAM in console.

  - Click on **roles** and then click on **Create Role**

  ![Fig 20](../image/IAMRoleGlue/1.png)

  - Under the **Use cases** drop down, search for Glue and select **Glue** and click on **Next**

  ![Fig 21](../image/IAMRoleGlue/2.png)

  - Search for **AmazonS3FullAccess**, **AmazonAthenaFullAccess**, **AWSGlueServiceRole** and select all of these and Click on **Next**

  ![Fig 22](../image/IAMRoleGlue/3.png)

  - Give the role name as **AWSGlueServiceRole-crawler-es**

  ![Fig 23](../image/IAMRoleGlue/4.png)
  
  - Click on **Create role**

  **Step to create table inside AWS Glue. First we need to create Database and then we will create table inside that or even you can use the database which get created with CloudFormation Template with the name energystorage1**

  **Example to create database-**

  **i.** Navigate to **Data Catalog**. From the side panel expand **Data Catlog** and click on **Database**

  ![Fig 24](../image/GlueDatabase/1.png)

  ![Fig 24](../image/GlueDatabase/1.5.png)

  **ii.** Click on **add Database** and provide the name of database as **energystorage**. Then click on **create database**.

  ![Fig 25](../image/GlueDatabase/2.png)

  ![Fig 26](../image/GlueDatabase/3.png)

  **iii.** Click on the database that you have just created with name **energystorage**

  **iv.** Inside the selected database, click on **Add table** button.

  ![Fig 27](../image/GlueDatabase/4.png)

  **v.** Fill the following details :
  
  - Provide the table name as **batteries_measurements**

  - select **energystorage** as database from the Dropdown

  ![Fig 28](../image/GlueDatabase/5.png)

  - Inside Include path: Browse for the path to your s3 folder **batteries_measurements** from the bucket that you have created

  ![Fig 28](../image/GlueDatabase/6.png)

  - select **CSV** as data format.

  - choose **Pipe(|)** as delimiter. 

  - Click on **next**

  ![Fig 29](../image/GlueDatabase/7.png)

  **vi.** Now Click on **edit Schema as JSON**

  ![Fig 30](../image/GlueDatabase/8.png)

  - Remove **[]** and Copy the given schema present in Glue schema folder with name **batteries_measurements** and click on **save** 

  ![Fig 30](../image/GlueDatabase/9.png)

  Click on **Next**  

  ![Fig 31](../image/GlueDatabase/10.png)

  Click on **create**. This will create your table with name batteries_measurements

  ![Fig 32](../image/GlueDatabase/11.png)

  **vii.** Now click on the table that is created with name **batteries_measurements** and click on **action** at top and click on **edit table**

  ![Fig 33](../image/GlueDatabase/12.png)
  
  Under the serialization lib paste this **org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe** and click on **Save**

  ![Fig 33](../image/GlueDatabase/13.png)



  **viii.** Now Navigate for Crawlers in left
  
  - Click on **Create Crawlers**

  ![Fig 34](../image/GlueDatabase/14.png)

  - Give the name as **betteriesmeasurement** and Click on **next**

  ![Fig 35](../image/GlueDatabase/15.png)

  **ix.** On Data source configuration, click on **Yes** then click on **Add tables** 

  ![Fig 36](../image/GlueDatabase/16.png)

  **x.** Select database as **energystorage**, select table as **batteries_measurements**. Click on **confirm** and then click on **Next**

  ![Fig 37](../image/GlueDatabase/17.png)

  - Under IAM Role, choose Role as **AWSGlueServiceRole-crawler-es**

  ![Fig 38](../image/GlueDatabase/18.png)

  - Click on **next** twice and click on **create Crawlers**

  ![Fig 39](../image/GlueDatabase/19.png)
  
  **Creating classifier for crawler**

  - Navigate to **classifier** from the left panel
    
    ![Fig 200](../image/ClassifierForCrawler/1.png)

  - Click on **add classifier**

    ![Fig 201](../image/ClassifierForCrawler/2.png)

  - Give classifier name as **batteries-measurement-classifier**, classifier type as **csv** and Column delimiter as **|**

    ![Fig 202](../image/ClassifierForCrawler/3.png)

  - In Column headings select **Has Headings** and provide value:
  
  **batteryid,measuredttm,ecapacitykwh,maxpowerkw,nomdcvoltagevolts,maxcurrentamperes,curchargelevelkwh,newchargelevelkwh,newcurrentamperes,newvoltagevolts,installlat,installlong,col13,col14,col15**

  - Provide Column datatypes as:
  
  **LONG,STRING,LONG,LONG,LONG,LONG,LONG,DOUBLE,DOUBLE,DOUBLE,DOUBLE,DOUBLE,LONG,STRING,STRING**

  Then click on **create**

  ![Fig 203](../image/ClassifierForCrawler/4.png)

  **Attaching classifier to crawler and run crawler**

  - Navigate to crawler from the laft panel

  - Click on crawler **betteriesmeasurement**

    ![Fig 204](../image/ClassifierForCrawler/5.png)

  - Go to classfier section and click on **Assign classfiers**

    ![Fig 205](../image/ClassifierForCrawler/6.png)

  - Select **batteries-measurement-classifier** and click on **confirm***

    ![Fig 206](../image/ClassifierForCrawler/7.png)

  - Now Select **Crawler** and Click on **Run Crawler** button

    ![Fig 40](../image/GlueDatabase/20.png)

  > [!NOTE]
  > Classifier creation and attaching to crawler is specific to this glue table only.


**Create the second Glue Table inside the same database***

  - Skip first 3 steps to create Database and follow the rest of step as above from **(iv - x)***

  - Changes required for this table:

    a. In step **v** provide the table name as **betteries_info**

    b. In step **v** browse for the path to your s3 folder **betteries_info** from the bucket that you have created

    c. In step **v** choose **Comma(,)** as delimiter

    d. In step **vi**, Remove **[]**, Copy the schema present in **Glue schema** folder with name **betteries_info** and click on save and then click on **Next**.

    e. In step **viii** give the Crawlers name as **batteriesinfo**

    f. In Step **x** choose table as **betteries-info**
    
    g. Run crawler

  - Now we have step function with execution flow as athena query and Lambda function which is picking results of athena query to process it. This lambda will process the data and send it to SQS and that SQS will trigger another lambda, which will send data to IOT core as shown in Architecture. The Lambda that processes Athena results is triggered by the state machine, and the Lambda that sends data to IoT Core will triggered by SQS.


 **Create Lambda Functions**

  1. Now to create the Lambda function to pick result of Athena query with the same name that is given in step function definition **es-storagetwin**

  - Navigate to Lambda : Go to the AWS Management Console and navigate to the **Lambda** service.

  - Create a Function : Click the **Create function** button.

    ![Fig 40](../image/LambdaFunction/1.png)

  - Choose **Author from Scratch** and Fill in the following details:

    a. Function name: Enter a name for your Lambda function as **es-storagetwin**

    b. Runtime: Choose the runtime as **Node.js 16.x**

    c. Click the **Create function** button.

    ![Fig 41](../image/LambdaFunction/2.png)

  - Now paste the code present in **Lambda Code** folder with name **es-storagetwin** inside **index.js** 

  ![Fig 42](../image/LambdaFunction/3.png)

  > [!NOTE]
  > Replace the **region** and **AccountNo** as shown in below example.

  ![Fig 41](../image/Examples/3.png)

  ![Fig 41](../image/Examples/4.png)

  - Click on **Deploy** after doing changes

  - Now go to the **configuration** tab and click on **permission** tab from left and click on **role** name.

  ![Fig 43](../image/LambdaFunction/4.png)

  - Now click on **add permission** and choose **attach policies**

  ![Fig 44](../image/LambdaFunction/5.png)

  - Search for following one by one and select all this :

    **AmazonSQSFullAccess**, **AmazonS3FullAccess**, **AmazonAthenaFullAccess**, **AWSLambdaBasicExecutionRole**, **AWSIoTDataAccess**, **AmazonKinesisFirehoseFullAccess** and now click on **add permission**. This will give required permission to your Lambda function.

  ![Fig 45](../image/LambdaFunction/6.png)

  2. Create second Lambda Function which will get triggered by SQS Queue

  - Follow the same step given as above. Give the name of Lambda as **es-storagetwinmeasurements**. For code please refer to **es-storagetwinmeasurements** file present in **Lambda Code** folder.

  > [!NOTE]
  > Change **region** and **IOT_CORE_ENDPOINT** in the code file like shown in below example

  ![Fig 100](../image/Examples/5.png)
  
  ![Fig 101](../image/Examples/6.png)

  - Steps to find **IOT_CORE_ENDPOINT**

    a. Navigate to **IOT CORE** from console.

    b. Click on **setting** in left panel

      ![Fig 102](../image/Examples/7.png)

    c. Copy the **endpoint** and replace it in your code file

      ![Fig 103](../image/Examples/8.png)

    d. Click on **deploy**.

  - Provide **same role** as above Lambda Function.


**Create an SQS Queue to receive message send by above lambda**

Create an SQS queue with the same name **es-batteriesformeasurements** used in lambda function code in line no 5, where the processed data will be sent.

  Steps to create an Amazon Simple Queue Service (SQS)

  - Navigate to SQS

  ![Fig 46](../image/SQS/1.png)

  - Click the **Create Queue** button.

  - Configure Queue Settings:
    
    a. Queue Name: Enter a name for your SQS queue as **es-batteriesformeasurements**

    ![Fig 47](../image/SQS/2.png)

    b. Leave rest of things as default

    c. Now click the **Create Queue** button at bottom

    ![Fig 48](../image/SQS/3.png)

  -  Go to **Lambda Triggers** tab

  ![Fig 49](../image/SQS/4.png)

  - Click on **Configure Lambda function trigger**
  
  - Now click on drop down and choose the second Lambda function that you have created with name **es-storagetwinmeasurements**. Then click on **Save**

  ![Fig 50](../image/SQS/5.png)


## Prerequisite Setup Steps for 2b

  **1. Now Create the Third Glue Table inside the same database**

  - Navigate to **Data Catlog** do the same procedure. Skip first 3 steps to create Database and follow the rest of step as above from **(iv - x)**

  - Changes required for this table:

    a. In step **v** provide the table name as **esrenewable_plants_info**

    b. In step **v** browse for the path to your s3 folder **renewable-plants-info** from the bucket that you have created

    c. In step **v** choose **Comma(,)** as delimiter

    d. In step **vi**, Remove **[]** Copy the given schema present in **Glue schema**s folder with name **esrenewable_plants_info** and click on **save** and then click on **Next**.

    e. In step **viii** give the Crawlers name as **esrenewable_plants_info**
    
    f. In Step **x** choose table as **esrenewable_plants_info**

    g. Run crawler.

  **2. Create AWS Step Function**

  This state machine should have steps that include invoking the Athena query and triggering Lambda functions.

  Here are the general steps to create state machine in Step Functions :

  **1.** Navigate to Step Functions

  - Go to the AWS Management Console and navigate to the **StepFunctions** service.

  **2.** Create a State Machine

  - Click the **Create state machine** button.

  - Choose **Blank** template and click on **Select**.

  - Click on edit icon from top left and change the name of step function to **es-renewabletwin-stepf**.

  - Now click on **Code** section.

  - To get the code please refer folder energy-storage. Inside energy-storage go to folder Step Function Code and copy the code inside **es-renewabletwin-stepf** and paste it. Then click on **Create**.

  > [!NOTE] 
  > Change the **[AccountNo]** with your AWS account number and also change the **[region]** where you want to deploy these resources. Must do this changes for upcoming step function in 2c as well.

  > [!NOTE]
  > Also change the **[BucketName]**.

  > [!NOTE]
  > It is recommended use same BucketName in all three step function to avoid multiple bucket creation.

  - Click on **Create**

  - Click on **confirm**.

  - Your state machine will get created now click on to the **IAM role arn**

  - Now click on **add permission** and choose **attach policies**

  - Search for following one by one and select all this :-
  **AmazonS3FullAccess**, **AmazonAthenaFullAccess**, **AmazonForecastFullAccess** and now click on **add permission**. This will give required permission to your step function.

  **3. Create Lambda Function**

  - Go to the AWS Management Console and navigate to the ***Lambda*** service.

  - Create a Function:

    a. Click the **Create function** button.

    b. Choose **Author from Scratch**.

    c. Fill in the following details:

      i. Function name: Enter a name for your Lambda function as **es-renewabletwin**

      ii. Runtime: Choose the runtime as **Node.js 16.x**

      iii. Click the **Create function** button.

      iv. Now paste the code present in file **es-renewabletwin** inside folder **Lambda Code** in index.js 

      > [!NOTE]
      > Replace **region** in the code file

      - click on **Deploy**

      v. Now go to the **configuration tab** and click on **url** under role name.

      vi. Now click on **add permission** and choose **attach policies**.

      vii. Search for following one by one and select all this :-
          **AmazonSQSFullAccess**, **AmazonS3FullAccess**, **AmazonAthenaFullAccess**, **AWSLambdaBasicExecutionRole**, **AWSIoTDataAccess**, **AmazonKinesisFirehoseFullAccess** and now click on add permission. This will give required permission to your Lambda function.

## Prerequisite Setup Steps for 2c

  **1. Now Create the Third Glue Table inside the same database**

  - Skip first 3 steps to create Database and follow the rest of step as above from **(iv - x)** from first step function configuration

  - Changes required for this table:

    a. In step **v** provide the table name as **esfeeders_info**

    b. In step **v** browse for the path to your s3 folder **feeders-info** from the bucket that you have created

    c. In step **v** choose **Comma(,)** as delimiter

    d. In step **vi**, Remove **[]** Copy the given schema present in **Glue schema** folder with name **esfeeders_info** and click on **save** and then click on **Next**.

    e. In step **viii** give the Crawlers name as **esfeeders_info**

    f. In Step **x** choose table as **esfeeders_info**

    g. Run crawler

  **2. Create AWS Step Function**

  Here are the general steps to create state machine in Step Functions :

  **1.** Navigate to Step Functions

  - Go to the AWS Management Console and navigate to the **StepFunctions** service.

  **2.** Create a State Machine

  - Click the **Create state machine** button.

  - Choose **Blank** template and click on **Select**.

  - Click on edit icon from top left and change the name of step function to **es-feedertwin-stepf**.

  - Now click on **Code** section.

  - To get the code please go to folder Step Function Code **es-feedertwin-stepf** present in extracted zip file, copy content of this file and paste it and click on **Create**.

  > [!NOTE] 
  > Change the **[AccountNo]** with your AWS account number and also change the **[region]** where you want to deploy these resources. 

  > [!NOTE]
  > Also change the **[BucketName]**.

  > [!NOTE]
  > It is recommended use same BucketName in all three step function to avoid multiple bucket creation.

  - Click on **Create**

  - Click on **confirm**.

  - Your state machine will get created now click on to the **IAM role arn**

  - Now click on **add permission** and choose **attach policies**

  - Search for following one by one and select all this :-
  **AmazonS3FullAccess**, **AmazonAthenaFullAccess**, **AmazonForecastFullAccess** and now click on **add permission**. This will give required permission to your step function.

  **3. Create Lambda Function**

  - Go to the AWS Management Console and navigate to the **Lambda** service

  - Create a Function:

    a. Click the **Create function** button

    b.  Choose **Author from Scratch**

    c. Fill in the following details:

      i. Function name: Enter a name for your Lambda function as **es-feedertwin**

      ii. Runtime: Choose the runtime as **Node.js 16.x**

      iii. Click the **Create function** button

      iv. Now paste the code present in file **es-feedertwin** inside folder **Lambda Code** in **index.js** and click on **Deploy**

      > [!NOTE]
      > Replace **AccountNo** and **region** in the code file

      v. Now go to the **configuration** tab and click on url under role name.

      vi. Now click on **add permission** and choose **attach policies**

      vii. Search for following one by one and select all this :
          **AmazonS3FullAccess**, **AmazonAthenaFullAccess**, **AWSLambdaBasicExecutionRole**,
          **AWSIoTDataAccess**, **AmazonKinesisFirehoseFullAccess**, **AmazonSQSFullAccess** and now click on add permission. This will give required permission to your Lambda function.

## To visualize the data you can do the quick sight set up for testing as well by following the step explained in quick sight set-up above QuickSight Section.

## Testing estorage-forecast-dataprep-ff step function

> [!IMPORTANT]
>  Run Crawler with name **batteriesforecast** before running **estorage-forecast-dataprep-ff** step function.

> [!IMPORTANT]
> Upload the data to this **ForecastOutputBucket/batteries-measurements-forecast/forecast-input** s3 folder before starting **estorage-forecast-dataprep-ff** step function. You will get the data from **forecast test data** inside **energy-storage** folder. For creating the predictor, large set historical data is required. So upload all the given files. For this you also need to create the same s3 folder structure to upload the data

> [!NOTE]
> If you want to create predictor again, manually you have to delete predictor which is already present in amazon forecast. Then run the step function[ estorage-forecast-dataprep-ff ] which will create new predictor and forecast.

> [!IMPORTANT]
> To visualize the workflow, start all step function and check the corrresponding data in Opensearch and QuickSight

## Step to run Step Function

- Navigate for step function. Open all step function one by one and click on **start execution**

- ![Fig 50](../image/Step-function/204.png)

- Click on **start execution**

- ![Fig 50](../image/Step-function/203.png)

- Now you can check data in OpenSearch and backed-up data in s3 firehose-output-bucket, within 2 to 3 minute

- Run this "GET /_search" command to check upcoming data in OpenSearch under DevTool

- ![Fig 50](../image/Step-function/205.png)

## Cleanup steps:

Step-1. Navigate for CloudFormation in Console.

Step-2. Select the stack that you have created.

![Fig 50](../image/Cleanup/200.png)

Step-3. select **Delete** on top and then click on **Delete**.

![Fig 50](../image/Cleanup/201.png)

This will delete all the created resource.



## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

## Contribution

**awssai@amazon.com**

**nehanejh@amazon.com**
