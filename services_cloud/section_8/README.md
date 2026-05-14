# AWS Data Anlytics

## AWS Glue

AWS Glue is a **serverless data integration service** that makes it easy for analytics users to discover, prepare, move, and integrate data from multiple sources.

With **AWS Glue**, you can discover and connect to more than 70 diverse data sources and manage your data in a centralized data catalog. You can **visually create, run, and monitor extract, transform, and load (ETL) pipelines to load data into your data lakes**. Also, you can immediately search and query cataloged data using **Amazon Athena, Amazon EMR, and Amazon Redshift Spectrum**.

### Glue Data Catalog
A **metadata** store containing table definitions, job definitions, and other control information for your ETL workflows.

### Glue Crawler
Programs that connect to data sources, **infer data schemas**, and **create metadata table definitions** in the **Data Catalog**.

### Glue ETL Jobs
The business logic to extract data from sources, transform it using Apache Spark scripts or Python scripts, and load it into targets.

### Glue Triggers
Mechanisms to initiate job runs based on schedules or events.

### Glue Studio
graphical interface that makes it easy to create, run, and monitor data integration jobs in AWS Glue. You can visually compose data transformation workflows and seamlessly run them on the Apache Spark–based serverless ETL engine in AWS Glue.


## Glue Workshop:

1.Sign in to the AWS Management Console and open the AWS S3 console at https://eu-west-3.console.aws.amazon.com/s3/home?region=eu-west-3#

2. Choose your AWS S3 bucket **objstr-sae---z2-ppd-saeawsgaiapprd-026090536189-your-alias**

3. Create **TPC** Folder.

![alt text](../images/image18.png)

### TPC Database and Tables:  
**TPC data** , is a public dataset used in this workshop. This is a synthetic dataset about products, customer information, demographics. Household details, inventory, web sales, promotions and returns.  
Following are the details to focus on for this workshop:

* web_sales
* web_page
* promotion
* item
* customer
* customer_address
* household_demographics
* income_band

The following diagram represents their relations for online sales:  

![alt text](../images/image19.png)


### Import the the data in s3 bucket: 

1. Select your S3 bucket **datalake-your-alias**
2. Select **TPC** Folder
3. Click **Upload**, Top-right button inside the bucket.
4. Click add **folders**
5.Click **Upload**

### Create TPC Database:
Now, navigate to AWS Glue console and click on the **Databases** option on the left and then click on **Add database** button.

Since we are planning to build the data base for TPC data, name the database as **tpcdb**.

![alt text](../images/image23.png)


### Create tables:  

In this workshop we use a **crawler** to populate the **AWS Glue Data Catalog with tables**. This is the primary method used by most AWS Glue users. A crawler can crawl multiple data stores in a single run. Upon completion, the crawler creates or updates one or more tables in your Data Catalog. 

In this exercise, we will create a new **AWS Glue Crawler** and populate the catalog in the **tpcdb** database you created in the previous steps.  


### Create Glue Crawler:

Navigate to AWS **Glue** console, click on **Data Catalog** section and select **Crawler** option.
Click on **create crawler** button. 
Enter name : **tpc-crawler**   
Entrer Tags: company=sae, teram=nephelion, projectname=training  
and click on **next**,  

Click on **Add a data source**, select **S3 data source**, add **S3 path** and select **Crawl all sub-folders** option. 

Click on **Add an S3 data source** and click on **next**. 

Select IAM role **GlueServiceRole** and click on **next**  

Select the target databsase : **tcpdb**, add table name prefix : **raw-**  and click on **next**.


Review the crawler settings and click on **create crawler** button.

Now, to run the crawler, click on **Run Crawler** button. 

* You will see that the crawler is sucessfully starting.  

* You can click refresh button to see the progress of this crawler.  

It will take a minute or so to populate the tpc database with tables. When it is done crawling you will notice number of tables added under **Table changes from last run column**. In this case 8 tables have been added. 

You can also verify these tables in the AWS Lake Formation console under Data catalog and Tables.  

## Athena Access:

Amazon Athena is an **interactive query service** that makes it easy to analyze data directly in Amazon Simple Storage Service **(Amazon S3)** using **standard SQL**.

* From the AWS Management console, search for **Athena** service

* From the Athena Get started page, choose Launch Query editor.

Now, we can access the tables, execute query ```SELECT * FROM "tpcdb"."raw-dl_tpc_customer" limit 10;``` from Athena editor.


## Athena Views:

Create **web_sales_page_v** view joinning **raw-dl_tpc_web_sales** and ***raw-dl_tpc_web_page**:

```
create view "tpcdb_axelt"."web_sales_page_v" as
select sales.*, page.wp_url from 
"tpcdb_axelt"."raw-dl_tpc_web_sales" sales
inner join
"tpcdb_axelt"."raw-dl_tpc_web_page" page
on ws_web_page_sk  = wp_web_page_sk ;
```

Now, we can access the tables, execute query ```SELECT * FROM  "tpcdb_axelt"."web_sales_page_v";``` from Athena editor.

