# How to Load Data Into the Hortonworks Sandbox

### Summary

The Hortonworks Sandbox is a fully contained Hortonworks Data Platform (HDP) environment. The sandbox includes the core Hadoop components, as well as all the tools needed for data ingestion and processing. You can access and analyze sandbox data with many Business Intelligence (BI) applications.

In this tutorial, we will load and review data for a fictitious web retail store in what has become an established use case for Hadoop: deriving insights from large data sources such as web logs. By combining web logs with more traditional customer data, we can better understand our customers, and also understand how to optimize future promotions and advertising.

## Prerequisites

*   [Hortonworks Sandbox 2.4](http://hortonworks.com/products/hortonworks-sandbox/#install)(installed and running)

## Outline

To load data into the Hortonworks sandbox, you will:

*   [Step 1: Download the sample data](#download-sample-data)
*   [Step 2: Upload the Data Files into the Sandbox](#upload-the-data-files-into-the-sandbox)
*   [Step 3: Create Hive Tables](#create-hive-tables)
*   [Step 4: Load data into new tables](#load-data-into-new-tables)
*   [Step 5: View and Refine the Data in the Sandbox](#view-and-refine-the-data-in-the-sandbox)
*   [Further Resources](#further-resources)

### Step 1: Download the Sample Data <a id="download-sample-data"></a>

You can download a set of sample data contained in a compressed (.zip) folder here:

[RefineDemoData.zip](https://s3.amazonaws.com/hw-sandbox/tutorial8/RefineDemoData.zip)

Save the sample data .zip file to your computer, then extract the files and unzip Omniture.0.tsv.gz, users.tsv.gz and products.tsv.gz.

**Note**: The extracted data files should have a .tsv file extension at the end.

### Step 2: Upload the Data Files into the Sandbox <a id="upload-the-data-files-into-the-sandbox"></a>

2.1) Select the `HDFS Files view` from the Off-canvas menu at the top. The HDFS Files view allows you to view the Hortonworks Data Platform(HDP) file store. The HDP file system is separate from the local file system. 

![](/assets/hello-hdp/HDFS_file_view_icon.png)
> **Note:** Must be logged in under a user with sufficient privileges to change permissions. In the image above, maria.dev1 may not have privileges to change permissions, so you may need to login under admin user.

2.2) Navigate to `/tmp`, create an **admin** folder

![](/assets/how-to-load-data-into-sandbox/68747470733a2f42f7777772e676f6f676c6564726976652e636f6d2f686f73742f30427a686c4f79776e4f7071386155744456453554576a497a516d633f7261773d74727565.png)

2.3) right click on admin and select **Permissions**:

2.4) Now we check the `Write buttons` and `modify recursively` and press save.

![](/assets/how-to-load-data-into-sandbox/68747470733a22f22f7777772e676f6f676c6564726976652e636f6d2f686f73742f30427a686c4f79776e4f7071384d33557a4e693171636e426a516d633f7261773d74727565.png)

2.5) Verify that the permissions look now like the image below:

![](/assets/how-to-load-data-into-sandbox/68747470733a2f2f7777772e676f6f676c6564726976652e636f6d2f686f73742f30427a686c4f79776e4f70713854475a716345395252316876596d633f7261773d74727565.png)

2.6) Now, we navigate to `/tmp/admin`, click on upload and browse the `Omniture.0.tsv`.

Repeat this procedure for `users.tsv` file and for `products.tsv`.

### Step 3: Create Hive tables <a id="create-hive-tables"></a>

3.1) Let’s open the `Hive  View` by clicking on the Hive button from the `views menu`.

![](/assets/how-to-load-data-into-sandbox/687474707133a2f2f7777772e676f6f676c6564726976652e636f6d2f686f73742f30427a686c4f79776e4f707138566c68794c575934576e6c574e324d3f7261773d74727565.png)

3.2) Create the tables users, products and omniturelogs.

~~~
create table users (swid STRING, birth_dt STRING, gender_cd CHAR(1)) ROW FORMAT DELIMITED
FIELDS TERMINATED by '\t' stored as textfile 
tblproperties ("skip.header.line.count"="1");
~~~

![](/assets/how-to-load-data-into-sandbox/users_query.png)

~~~
create table products (url STRING, category STRING) ROW FORMAT DELIMITED
FIELDS TERMINATED by '\t' stored as textfile 
tblproperties ("skip.header.line.count"="1");
~~~

![](/assets/how-to-load-data-into-sandbox/products_query.png)

~~~
create table omniturelogs (col_1 STRING,col_2 STRING,col_3 STRING,col_4 STRING,col_5 STRING,col_6 STRING,col_7 STRING,col_8 STRING,col_9 STRING,col_10 STRING,col_11 STRING,col_12 STRING,col_13 STRING,col_14 STRING,col_15 STRING,col_16 STRING,col_17 STRING,col_18 STRING,col_19 STRING,col_20 STRING,col_21 STRING,col_22 STRING,col_23 STRING,col_24 STRING,col_25 STRING,col_26 STRING,col_27 STRING,col_28 STRING,col_29 STRING,col_30 STRING,col_31 STRING,col_32 STRING,col_33 STRING,col_34 STRING,col_35 STRING,col_36 STRING,col_37 STRING,col_38 STRING,col_39 STRING,col_40 STRING,col_41 STRING,col_42 STRING,col_43 STRING,col_44 STRING,col_45 STRING,col_46 STRING,col_47 STRING,col_48 STRING,col_49 STRING,col_50 STRING,col_51 STRING,col_52 STRING,col_53 STRING) ROW FORMAT DELIMITED
FIELDS TERMINATED by '\t' stored as textfile 
tblproperties ("skip.header.line.count"="1");
~~~

![](/assets/how-to-load-data-into-sandbox/omniturelogs_query.png)

### Step 4: Load data into new tables <a id="load-data-into-new-tables"></a>

4.1) To load the data into the tables, we have to execute the following queries.

~~~
LOAD DATA INPATH '/tmp/admin/products.tsv' OVERWRITE INTO TABLE products; 
LOAD DATA INPATH '/tmp/admin/users.tsv' OVERWRITE INTO TABLE users; 
LOAD DATA INPATH '/tmp/admin/Omniture.0.tsv' OVERWRITE INTO TABLE omniturelogs;
~~~

![](/assets/how-to-load-data-into-sandbox/load_data_newTables.png)

4.2) To check if the data was loaded, click on the icon next to the table name. It executes a sample query.

![](/assets/how-to-load-data-into-sandbox/products_data_table.png)  
![](/assets/how-to-load-data-into-sandbox/users_data_table.png)  
![](/assets/how-to-load-data-into-sandbox/omniturelog_data_table.png)

### Step 5: View and Refine the Data in the Sandbox <a id="view-and-refine-the-data-in-the-sandbox"></a>

In the previous section, we created sandbox tables from uploaded data files. Now let’s take a closer look at that data.

Here’s a summary of the data we’re working with:

**omniturelogs** – website logs containing information such as URL, timestamp, IP address, geocoded IP, and session ID.

![](/assets/how-to-load-data-into-sandbox/687474707133a2f2f7777772e676f6f676c6564726976652e636f6d2f686f73742f30427a686c4f79776e4f707138516e70445a4846365332395454584d3f7261773d74727565.png?dl=1)  
![](/assets/how-to-load-data-into-sandbox/68747470733a2f2f7777772e676f6f676c6564726976652e636f6d2f686f73742f30427a686c4f79776e4f7071386158707a5430787161475a6d5647383f7261773d74727565.png?dl=1)

**users** – CRM user data listing SWIDs (Software User IDs) along with date of birth and gender.

![](/assets/how-to-load-data-into-sandbox/68747470733a2f2f7777772e676f6f676c6564726976652e636f6d2f686f73742f30427a686c4f79776e4f70713854576b784d46677757576c465344513f7261773d74727565.png?dl=1)

**products** – CMS data that maps product categories to website URLs.
> **Note:** your products image should look similar to users image above.

5.1) Now let’s use a Hive script to generate an “omniture” view that contains a subset of the data in the Omniture log table.

~~~
CREATE VIEW omniture AS  
SELECT col_2 ts, col_8 ip, col_13 url, col_14 swid, col_50 city, col_51 country, col_53 state 
FROM omniturelogs
~~~

![](/assets/how-to-load-data-into-sandbox/omniture_subsetview_query.png)

5.2) Click Save as. On the “Saving item” pop-up, type “omniture” in the box, then click OK.

![](/assets/how-to-load-data-into-sandbox/68747470733a2f2f7777772e676f6f676c6564726976652e636f6d2f686f73742f30427a686c4f79776e4f707138546c52745557684d5130686c5645453f7261773d74727565.png?dl=1)

5.3) You can see your saved query now by clicking on the “Save Queries” button at the top.

![](/assets/how-to-load-data-into-sandbox/68747470733a2f2f7777772e676f6f676c6564726976652e636f6d2f686f73742f30427a686c4f79776e4f707138556c59305758426b623164526157633f7261773d74727565.png?dl=1)

5.4) Click Execute to run the script.

To view the data generated by the saved script, click on the icon next to the view’s name at the Database Explorer.  
The query results will appear, and you can see that the results include the data from the omniturelogs table that were specified in the query.

![](/assets/how-to-load-data-into-sandbox/omniture_data_table.png)

5.5) Finally, we’ll create a script that joins the omniture website log data to the CRM data (registered users) and CMS data (products). Click Query Editor, then paste the following text in the Query box:

~~~
create table webloganalytics as select to_date(o.ts) logdate, o.url, o.ip, o.city, upper(o.state) state, o.country, p.category, CAST(datediff( from_unixtime( unix_timestamp() ), from_unixtime( unix_timestamp(u.birth_dt, 'dd-MMM-yy'))) / 365 AS INT) age,  u.gender_cd from omniture o inner join products p on o.url = p.url left outer join users u on o.swid = concat('{', u.swid , '}')
~~~

5.6) Save this script as “webloganalytics” and execute the script.

![](/assets/how-to-load-data-into-sandbox/68747470733a2f2f7777772e676f6f676c6564726976652e636f6d2f686f73742f30427a686c4f79776e4f707138526c6877583246664d3367774e55453f7261773d74727565.png?dl=1)

You can view the data generated by the script as described in the preceding steps.

![](/assets/how-to-load-data-into-sandbox/webloganalytics_data_table.png)

Now that you have loaded data into the Hortonworks Platform, you can use Business Intelligence (BI) applications such as Microsoft Excel to access and analyze the data.  

## Further Resources <a id="further-resources"></a>
- [How to Use Excel 2013 to Access Hadoop Data](http://hortonworks.com/hadoop-tutorial/how-to-use-excel-2013-to-access-hadoop-data/)
- [How to Use Excel 2013 to Analyze Hadoop Data](http://hortonworks.com/hadoop-tutorial/how-use-excel-2013-to-analyze-hadoop-data/)