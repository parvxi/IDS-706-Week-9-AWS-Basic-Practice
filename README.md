# AWS Serverless Data Lake Workshop

**IDS 706: Data Engineering Systems | Duke University | Fall 2026**

## Overview

For this week's AWS workshop, I got to work with different AWS services and see how they connect to build a serverless data lake. I started by streaming data into Amazon S3, then used AWS Glue to discover its schema and transform the data. I also worked with a Jupyter Notebook for interactive ETL development and used Amazon Athena to query a much larger dataset.

What I found interesting was seeing how the different services fit together, from collecting raw data to preparing and analyzing it, without having to manage any servers.

## 1. Real-Time Data Streaming into Amazon S3

I used Amazon Kinesis Data Firehose to stream data into an S3 bucket. Firehose handled delivering the incoming records, and I was able to verify that the files were being stored in S3.

The data was organized into folders by year, month, day, and hour, which makes it easier to manage and locate records.

**Firehose delivery stream**

![Amazon Kinesis Data Firehose](AWS%20Screenshots/01-firehose.png)

**Raw data stored in S3**

![Raw data in Amazon S3](AWS%20Screenshots/02-s3-raw.png)

## 2. Discovering the Schema with AWS Glue Crawler

Next, I created and ran an AWS Glue crawler to automatically discover the structure of the data stored in S3.

The crawler created a table called `raw` in the `sdl-demo-data` database. I checked the generated schema and confirmed that the partition columns (`year`, `month`, `day`, and `hour`) were correctly identified.

**Glue crawler execution**

![AWS Glue Crawler](AWS%20Screenshots/03-glue-crawler.png)

**Discovered table schema**

![AWS Glue Table Schema](AWS%20Screenshots/04-glue-schema.png)

## 3. Transforming Data with AWS Glue Studio

In this part, I used the visual ETL editor in AWS Glue Studio to create a transformation job called `transform-json-to-parquet`.

The job performed three main transformations:

- Converted the raw JSON data into Parquet format with Snappy compression.
- Renamed `datesoldsince` to `date_start` and `datesolduntil` to `date_until`.
- Removed the `color` column.

After running the job, I verified that it completed successfully and generated 20 Parquet files in the `compressed-parquet/` S3 folder.

**Successful Glue transformation job**

![AWS Glue Job](AWS%20Screenshots/05-glue-job.png)

**Parquet output in S3**

![Parquet Output](AWS%20Screenshots/06-parquet-output.png)

## 4. Interactive ETL Development with Jupyter Notebook

I also explored AWS Glue interactive sessions through a Jupyter Notebook. Unlike the visual editor, the notebook allowed me to work directly with Python and PySpark to load, inspect, transform, and write data.

I practiced repartitioning the data into a single partition and partitioning it by department. I then checked the output files in S3 to confirm that the transformations had written successfully.

**Notebook setup and data loading**

![Jupyter Notebook Setup](AWS%20Screenshots/07-notebook-setup.png)

**Data transformation code**

![Jupyter Notebook Transformations](AWS%20Screenshots/08-notebook-transformations.png)

**Output files organized by department**

![Department Partition Output](AWS%20Screenshots/09-notebook-s3-byDepartment.png)

**Single-partition output**

![Single Partition Output](AWS%20Screenshots/10-single-partition.png)

## 5. SQL Analytics with Amazon Athena

For the final lab, I used Amazon Athena to explore the GDELT dataset, which contains information about events reported in global news.

I created a database called `gdelt` and added five tables: `events`, `eventcodes`, `countries`, `groups`, and `types`.

I then ran SQL queries to explore the dataset, including counting events by year, identifying the most common event categories, and analyzing events involving Barack Obama and Angela Merkel.

One result I found interesting was that the event category **"Make statement"** appeared more than 46 million times in the dataset.

This part helped me understand how Athena can query large datasets stored in S3 using SQL, without having to move the data into a traditional database.

**GDELT database and tables**

![Athena Database](AWS%20Screenshots/11-athena-database.png)

**Athena SQL query results**

![Athena Query Results](AWS%20Screenshots/12-athena-query-results.png)

## What I Learned

This workshop helped me understand how AWS services work together in a data engineering pipeline. Before this, I had mostly practiced data transformation and SQL separately, so it was useful to see how streaming, storage, cataloging, transformation, and analytics connect in one workflow.

I also got more comfortable navigating AWS, working with S3 paths, using Glue for ETL, and querying data through Athena.

Overall, it was a good hands-on introduction to building and working with a serverless data lake.
