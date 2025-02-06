# Amazon Books DAG - README

## Overview
This Apache Airflow DAG fetches book data from Amazon and stores it in a PostgreSQL database. The workflow consists of three tasks:

1. **Fetch Book Data**: Scrapes book details from Amazon search results.
2. **Create Table in PostgreSQL**: Ensures that the required `books` table exists in the PostgreSQL database.
3. **Insert Book Data**: Inserts the fetched book data into the PostgreSQL database.

## Prerequisites

### Airflow Setup
Ensure that Apache Airflow is installed and running. You should have a configured Airflow instance with PostgreSQL connection setup.

### Database Connection
The DAG relies on a PostgreSQL connection named `books_connection`. Ensure that this connection is properly set up in Airflow.

### Required Python Packages
The following Python packages are required for the DAG:
- `requests`
- `pandas`
- `beautifulsoup4`
- `apache-airflow-providers-postgres`

Install them using:
```sh
pip install requests pandas beautifulsoup4 apache-airflow-providers-postgres
```

## DAG Details

### DAG Configuration
- **DAG Name**: `fetch_and_store_amazon_books`
- **Start Date**: February 1, 2025
- **Schedule Interval**: Daily
- **Retries**: 1 (with a delay of 5 minutes)

### Task Breakdown

#### 1. Fetch Book Data
- Scrapes Amazon for up to 50 data engineering book listings.
- Extracts Title, Author, Price, and Rating.
- Stores the extracted data in an Airflow XCom.

#### 2. Create Table in PostgreSQL
- Ensures that the `books` table exists before inserting data.
- Table schema:
  ```sql
  CREATE TABLE IF NOT EXISTS books (
      id SERIAL PRIMARY KEY,
      title TEXT NOT NULL,
      authors TEXT,
      price TEXT,
      rating TEXT
  );
  ```

#### 3. Insert Book Data
- Retrieves book data from XCom.
- Inserts each record into the PostgreSQL `books` table.

## How to Use
1. Place the DAG file in your Airflow `dags` directory.
2. Start Airflow services:
   ```sh
   airflow scheduler & airflow webserver
   ```
3. Enable the DAG from the Airflow UI.
4. Check logs and XCom to verify data extraction and insertion.

## Troubleshooting
- **Failed Requests**: Ensure your network allows outbound connections to Amazon.
- **Scraping Issues**: Amazon's page structure may change, requiring updates to the parsing logic.
- **Database Errors**: Check if `books_connection` is correctly set up in Airflow.

## Future Improvements
- Implement error handling and logging.
- Store timestamps to track book data updates.
- Use an API for fetching book data instead of web scraping.

---


