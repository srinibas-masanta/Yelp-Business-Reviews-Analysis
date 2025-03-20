# Yelp Business Reviews Analysis

## Project Overview
This project conducts an end-to-end data analytics on Yelp business reviews using Python, Snowflake, and SQL. The dataset, consisting of over 7 million reviews in a 5GB JSON file, presents challenges in ingestion, transformation, and analysis. We tackle these challenges by optimizing data processing, leveraging cloud storage, and performing insightful SQL queries.

## Dataset
- **Size:** 5GB JSON dataset with **7 million rows**.
- **Source:** [Yelp Open Dataset](https://business.yelp.com/data/resources/open-dataset/).
- **Files:** `yelp_academic_dataset_review.json`, `yelp_academic_dataset_business.json`.

## Workflow Details
### 1. Data Preprocessing & Splitting
- The original JSON file is too large for direct processing.
- We use **Python** to split it into smaller chunks (~500MB each) to optimize the upload process.

### 2. Uploading to Amazon S3
- Created an **IAM user** in AWS with S3 Read-Only Access.
- Generated an **Access Key** for authentication.
- Uploaded split JSON files to an **S3 bucket** for efficient storage and retrieval.

### 3. Ingesting Data into Snowflake
- Used `COPY INTO` command to **load data from S3 into Snowflake**.
- Splitting the files enables **faster ingestion**.

**Better Alternative**: Use IAM Roles Instead of Hardcoding Keys  
Instead of embedding AWS credentials directly, consider using **IAM roles** (if using AWS) or **external stages** in Snowflake:  

```sql
COPY INTO yelp_reviews
FROM 's3://yelp-v1/my-files/'
STORAGE_INTEGRATION = my_s3_integration
FILE_FORMAT = (TYPE = JSON);
```

### 4. Data Transformation & Processing in Snowflake
- Converted JSON data into a structured **tabular format**.
- Implemented **Python UDFs** to classify reviews as **Positive, Negative, or Neutral**.

### 5. SQL-Based Analysis & Insights
We ran **SQL queries** to extract valuable insights, including:
- Business count per category.
- Top users reviewing restaurants.
- Most popular business categories based on reviews.
- Businesses with the highest percentage of **5-star reviews**.
- Monthly review trends.
- Sentiment-based business rankings.

## Notebooks & Workbooks
The following files are included in the repository:
- **yelp_files_split.ipynb** → Python notebook for splitting JSON files.
- **yelp_reviews.txt** → Snowflake queries for review data ingestion.
- **yelp_business.txt** → Snowflake queries for business data ingestion.
- **sentiments.txt** → Snowflake UDF for sentiment analysis.
- **SQL_Questions.txt** → SQL queries for analysis and insights.

## Technologies Used
- **Python** (json)
- **Amazon S3** (cloud storage)
- **Snowflake** (data warehouse, SQL queries, Python UDFs)
- **SQL** (data transformation, analysis, insights)

## Suggestions for Enhancement
- **Optimize Sentiment Analysis**: Use **NLTK/VADER** or **BERT-based models** instead of TextBlob for more accurate sentiment classification.
- **Leverage Snowflake ML Functions**: Integrate **machine learning models** within Snowflake for scalable predictions.
- **Visualization Dashboard**: Use **Streamlit, Power BI, or Tableau** to create an interactive dashboard for insights.
- **Automate Data Pipeline**: Implement **AWS Lambda or Airflow** to automate data ingestion and transformation.
