# HappyTimes Architecture Document

![image](https://github.com/sarathsagi/dataeng/assets/13203694/81261c7e-39bd-46af-8e94-e70884b415d5)


## Data Sources:

- **RDBMS**: The system collects data from relational databases using the Debezium platform for change data capture (CDC). This data is streamed to Apache Kafka for real-time processing and analytics.

- **Log Files**: Raw log data is collected by the Log Collector, categorized into different types, and then processed using Spark for analysis and storage in the Data Lake.

- **Click Events**: Application clients produce various events, including user interactions, geolocation data, and more. These events are ingested into Apache Kafka, transformed, and prepared for storage in the Data Lake.

- **SFTP Servers**: Data is retrieved from remote SFTP servers using Python with libraries like pysftp or paramiko. The downloaded files are uploaded to a data lake destination such as AWS S3 or HDFS, ensuring periodic extraction of data for analysis and processing.

## Data Processing Pipelines:

### RDBMS Data to Data Lake:
- Debezium captures changes in the RDBMS and streams them to Apache Kafka.
- Kafka consumer applications process and analyze the data in real-time or store it in the Data Lake.

### Log Files to Data Lake:
- Log Collector categorizes raw log data into different types based on website actions.
- Spark processes and organizes the data for analysis and storage.

### Click Events to Data Lake:
- Application clients generate events that are transmitted to Apache Kafka.
- Transformations standardize, enrich, and aggregate the events, preparing the data for storage in the Data Lake.

### SFTP Servers to Data Lake:
- Python with libraries like pysftp or paramiko is used to establish an SFTP connection to retrieve data.
- The downloaded files are uploaded to a data lake using libraries like boto3 or pywebhdfs, ensuring automation, error handling, and logging for a seamless data transfer process.

# Data Modeling

Data modeling with DBT (Data Build Tool) is a method for transforming and structuring data in modern data stacks. It involves defining SQL-based transformation models, specifying materializations (e.g., tables or views), and writing tests and documentation to ensure data quality and clarity. DBT automates the execution of these transformations, can be scheduled for regular updates, and supports version control and CI/CD for managing changes. The result is a streamlined, collaborative approach to data transformation and modeling, making data analysis more efficient and manageable.

## Dimension Tables:

### dim_visitor

- **Description**: This table stores information about individual visitors, including attributes like visitor ID, Name, location, device type, and source of visit (email, social, SEM).

- **Columns**:
  - visitor_id (Primary Key)
  - visitor_name
  - location
  - device_type
  - source (email, social, SEM)

### dim_content

- **Description**: Contains details about the content on your site, such as content ID, type (article, video, etc.), title, author, and category.

- **Columns**:
  - content_id (Primary Key)
  - content_type
  - content_title
  - author
  - Category

### dim_ad_affiliate

- **Description**: Includes information about ads and affiliate links, such as ad/affiliate ID, campaign, source, and ad type.

- **Columns**:
  - ad_affiliate_id (Primary Key)
  - campaign
  - source
  - ad_type

### dim_subscription

- **Description**: Stores data related to subscriptions, including subscriber ID, subscription type, start date, and end date.

- **Columns**:
  - subscription_id (Primary Key)
  - subscriber_id
  - subscription_type
  - start_date
  - end_date

### dim_social_media

- **Description**: Contains information about social media outlets, such as outlet ID, outlet name, and sentiment category (positive, negative, neutral).

- **Columns**:
  - social_media_id (Primary Key)
  - social_media_name
  - sentiment_category

## Fact Tables:

### fact_visitor_activity

- **Description**: This table captures visitor interactions with your site, including page views, clicks, and other relevant activities. Fields may include visitor ID, date, content ID, and source.

- **Columns**:
  - activity_id (Primary Key)
  - visitor_id (Foreign Key)
  - date_id (Foreign Key)
  - content_id (Foreign Key)
  - source
  - activity_type (page view, click, etc.)

### fact_conversions

- **Description**: Tracks conversions and conversion rates. It includes data on which ads or affiliate links were clicked, and if the visitor subsequently subscribed. Fields may include visitor ID, date, ad/affiliate ID, and subscription status.

- **Columns**:
  - conversion_id (Primary Key)
  - visitor_id (Foreign Key)
  - date_id (Foreign Key)
  - ad_affiliate_id (Foreign Key)
  - subscription_status (converted or not)

### fact_retentions

- **Description**: Records site retention and return visits. It should include visitor ID, date, and a flag indicating whether it was a return visit.

- **Columns**:
  - retention_id (Primary Key)
  - visitor_id (Foreign Key)
  - date_id (Foreign Key)
  - return_visit (yes or no)

### fact_social_sentiment

- **Description**: Logs social media sentiment data. It includes outlet ID, date, and sentiment score.

- **Columns**:
  - sentiment_id (Primary Key)
  - social_media_id (Foreign Key)
  - date_id (Foreign Key)
  - sentiment_score


## Funnel Analysis:

- **Awareness**: Count the number of visitors by source.

- **Consideration**: Find the most popular content among subscribers.

- **Conversion**: Calculate conversion rates for each ad/affiliate campaign.

- **Loyalty**: Track retention rates over time.

- **Advocacy**: Analyze social sentiment on a specific social media outlet.

## Data Orchestration:

Data pipeline orchestration is performed using Apache Airflow. It involves breaking down the pipeline into tasks, defining task dependencies, scheduling when the pipeline should run, and monitoring task execution. Airflow offers features such as parallel execution, dynamic workflows, and error handling, making it a versatile choice for orchestrating data pipelines.

## Data Observability:

Data observability is essential for maintaining data quality, tracking data lineage, and promptly detecting anomalies or issues in the data pipeline. Data observability tools enhance collaboration, support data governance, and provide a basis for trust in the insights derived from the data pipeline.
