# Energy Storage Monitoring and Grid Integration Planning

## Architecture Overview

![Fig 1111](../image/general_arch.png)

## Project overview

This solution is to maximize grid benefits with Energy storage and Grid integration Planning. Project focus on energy storage monitoring, which is an emerging area within the power industry. Using Energy Storage helps us to add more renewable energy to our power grid. If we can see how much energy we have stored, how much renewable energy we are making, and how fulls power lines are, it helps the companies that provide our electricity make better plans for using renewable energy.
Actual data will flow from various sources, including batteries, wind turbine, solar panels and meter.

## Prerequisite to deploy the template

  - Create an user with **OpenSearch** full access

  - For this navigate to IAM Console and click on **User** in left navigation pane

  ![Fig 65](image/IAMAdminAccessUser/1.png)

  - Click on **Create User**

  - Provide user name and click on **Next**

  ![Fig 66](image/IAMAdminAccessUser/2.png)

  - Select **Attach Policy** Directly and then search for **AmazonOpenSearchServiceFullAccess** and select it

  ![Fig 67](image/IAMAdminAccessUser/3.png)

  - Click on **Next** 
  
  - Click on **Create User**

  ![Fig 666](image/IAMAdminAccessUser/4.png)

## Ways to run CloudFormation script


| AWS Region Code | Name | Launch |
| --- | --- | --- 
| ap-south-1 |India (Mumbai)| [![cloudformation-launch-stack](image/architecture/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-south-1#/stacks/new?stackName=Energy-Storage&templateURL=https://aws-refarch.s3.amazonaws.com/energy-storage/v1/templates/00-main.yaml) |
| us-east-1 |US East (N. Virginia)| [![cloudformation-launch-stack](image/architecture/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=Energy-Storage&templateURL=https://aws-refarch.s3.amazonaws.com/energy-storage/v1/templates/00-main.yaml) |
| us-east-2 |US East (Ohio)| [![cloudformation-launch-stack](image/architecture/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=Energy-Storage&templateURL=https://aws-refarch.s3.amazonaws.com/energy-storage/v1/templates/00-main.yaml) |
| us-west-2 |US West (Oregon)| [![cloudformation-launch-stack](image/architecture/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=Energy-Storage&templateURL=https://aws-refarch.s3.amazonaws.com/energy-storage/v1/templates/00-main.yaml) |
| ap-southeast-1 |AP (Singapore)| [![cloudformation-launch-stack](image/architecture/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/new?stackName=Energy-Storage&templateURL=https://aws-refarch.s3.amazonaws.com/energy-storage/v1/templates/00-main.yaml) |


**1. From Cloudformation Service in AWS**

  - Click on **Launch stack** button in region of your choice.

  ![Fig 61](image/CloudformationStackDeployement/1.png)

  - Click on **Next**

  ![Fig 62](image/CloudformationStackDeployement/2.png)

  - Specify Stack Details:

    > [!NOTE]
    > All bucket name should be globally unique and bucket should contain only lowercase and  hyphon.

    > [!NOTE]
    > Please provide strong password for OpenSearch, with atleast one Uppercase, one Lowercase, one number and one Special character

     i. FireHoseOutputBucket : this bucket is for storing backup of all the data send by Firehose

     ii. ForeCastOutputBucket : this bucket is for storing forecast output

     iii. IAMUserName : Your IAM username that you have created with OpenSearch full access 

     iv. OpenSearchUserName : Your username to login OpenSearch

     v.  OpenSearchUserPassword : Your password 



     Click on **Next**

      ![Fig 62](image/CloudformationStackDeployement/3.png)

  - Click on **Next**

  ![Fig 624](image/CloudformationStackDeployement/4.png)
  
  - Click on check box - 
  
  **I acknowledge that AWS CloudFormation might create IAM resources with custom names**

  **I acknowledge that AWS CloudFormation might require the following capability: CAPABILITY_AUTO_EXPAND**

  Once you're satisfied Click on **Submit** button

  - Stack Creation Progress: You'll be redirected to the stack's management console page where you can monitor the stack creation progress. It will take around 30 minute to complete stack creation with all the resources.

  ![Fig 64](image/CloudformationStackDeployement/5.png)



## Open Search Setup

  - Navigate for OpenSearch Click on domain **meter-data-analysis**.

  ![Fig 68](image/OpenSearch/1.png)

  - Go to **security configuration** tab

  ![Fig 69](image/OpenSearch/2.png)
  
  - click on **edit**

  - provide **master username**, **password** and **confirm password**

  ![Fig 70](image/OpenSearch/3.png)

  - Scroll down and choose→ **Only use fine-grained access control** under **Access policy**
  
  ![Fig 700](image/OpenSearch/18.png)

  - Click on **clear policy** and then **save changes**

  ![Fig 700](image/OpenSearch/4.png)

  ![Fig 71](image/OpenSearch/16.png)

  - Now Login to OpenSearch by clicking on **OpenSearch Dashboards URL** 

  ![Fig 72](image/OpenSearch/5.png)

  - Log In with created master username and password
  
  ![Fig 72](image/OpenSearch/19.png)

  - Click on **Confirm**

  ![Fig 72](image/OpenSearch/20.png)

  - Click on **Interact with OpenSearch API**

  ![Fig 72](image/OpenSearch/21.png)


  - **Now we will map the fireshose backend role to OpenSearch**

 **i.**  Go to side panel and click on **Security**

  ![Fig 73](image/OpenSearch/6.png)

**ii.** Click on **Roles** in side panel

  ![Fig 74](image/OpenSearch/7.png)

**iii.** Click on **all_access**

**iv.**  Go to **Mapped users** tab and click on **manage mappings**

 ![Fig 75](image/OpenSearch/8.png)

**v.** Now in another tab navigate for Firehose, to get the **Firehose role arn**. Open first Firehose with name **estorage-batteries-measurements-openSearch**.

  ![Fig 76](image/OpenSearch/17.png)

**vi.** Go to **Configuration** tab.

  ![Fig 766](image/OpenSearch/9.png)

**vii.**  scroll down to get IAM role and click on role **URL**.

  ![Fig 77](image/OpenSearch/10.png)

**viii.** Copy **ARN** of Firehose and **paste** this arn under **backend role** where you left in previous tab. 

  ![Fig 78](image/OpenSearch/11.png) 

**ix.** Repeat this step to map all three Firehose role ARN to OpenSearch by clicking on **Add another backend role** . Then click on **Map**
  ![Fig 788](image/OpenSearch/12.png)

  
**Now create OpenSearch index**

**i.** Go to side panel and click on **Dev Tools**

  ![Fig 79](image/OpenSearch/13.png)

**ii.** [Click here](docs/README_INDEX.md) to get the command to create index in OpenSearch. Paste this code and click on run button

![Fig 80](image/OpenSearch/14.png)

![Fig 81](image/OpenSearch/15.png)

**iv.** Command to search for data, once data dumped from device:

      GET /_search


**To Visualise data in QuickSight**

 [Click this link for step by step guide](docs/README_QUICKSIGHT.md).

## In case if you want to test it by generating sample data then prerequisites for these involves a series of steps on the AWS Console.**

 [Click this link for step by step guide](docs/README_TESTING.md).

 ## Contribution

**awssai@amazon.com**

**nehanejh@amazon.com**
