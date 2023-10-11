# HappyTimes Architecture Document

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

## Data Modeling:

Data modeling is performed using DBT (Data Build Tool) to structure and transform data. This includes defining SQL-based transformation models, specifying materializations, writing tests, and documentation to ensure data quality. DBT automates these transformations, supports version control, and CI/CD for efficient data transformation and modeling.

### Dimension Tables:

- **Visitor Dimension**: Stores information about individual visitors, including attributes like visitor ID, Name, location, device type, and source of visit (email, social, SEM).

- **Content Dimension**: Contains details about the content on your site, such as content ID, type, title, author, and category.

- **Ad/Affiliate Dimension**: Includes information about ads and affiliate links, such as ad/affiliate ID, campaign, source, and ad type.

- **Subscription Dimension**: Stores data related to subscriptions, including subscriber ID, subscription type, start date, and end date.

- **Social Media Dimension**: Contains information about social media outlets, such as outlet ID, outlet name, and sentiment category.

### Fact Tables:

- **Visitor Activity Fact Table**: Captures visitor interactions with your site, including page views, clicks, and other activities.

- **Conversion Fact Table**: Tracks conversions and conversion rates, including data on clicked ads or affiliate links and subscription status.

- **Retention Fact Table**: Records site retention and return visits, indicating whether it was a return visit.

- **Social Sentiment Fact Table**: Logs social media sentiment data, including outlet ID, sentiment category, and sentiment score.

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
